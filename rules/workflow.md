# Workflow Orchestration

## Build vs Buy

Before building a new product, substantial feature, or significant component, **before writing any implementation code**:

1. Search for existing open-source libraries or frameworks that may supply some of the required functionality. For substantial functionality, also consider software as a service.
2. Identify the top 5 candidates based on fit, GitHub stars, maintenance activity, and community adoption (or fit, popularity, cost, and reliability for services).
3. Present a comparison table with:
   - License
   - Pros (up to 5 key strengths)
   - Cons (up to 5 key weaknesses or risks)
   - Fit assessment for this specific use case
   Include information regarding price, licensing, and other pertinent details as needed.
4. Ask which option is preferred (including "build from scratch") before proceeding.

Skip this step only when explicitly told "build from scratch" or "no library search needed."

## Plan Mode Default

- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately — don't keep pushing
- Use plan mode for verification steps, not just building
- Treat undocumented infrastructure choices as architectural decisions — require explicit confirmation before encoding them in code or config

## Subagent Strategy

### Agent Naming

**Orchestrators and team leads** (agents invoked directly by the user) MUST run `/rename <descriptive-task-name>` immediately upon starting a task — before any other work. This allows the user to resume the session after a crash or reboot. The name should describe the task, e.g., `auth-refactor`, `fix-login-bug`, `migrate-db-schema`.

**Subagents and teammates** (agents spawned by an orchestrator) must be given names that make their role unambiguous so the user knows not to attempt to resume those sessions. Use a `sub/` or `team/` prefix in the `name` parameter when invoking the Agent tool:
- Subagents: `sub/research-auth-libraries`, `sub/run-tests`, `sub/explore-schema`
- Teammates: `team/implement-api`, `team/write-tests`, `team/review-output`

- Use subagents liberally to keep the main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

### ABSOLUTE RULES — NO EXCEPTIONS

> **🚫 NEVER WORK IN THE MAIN PROJECT ROOT.** Subagents and teammates MUST use worktree isolation (`isolation: "worktree"`) for ANY work that touches files. There is NO scenario where a subagent or teammate should be editing files, running builds, or making changes directly in the main project directory. The main project root is the orchestrator's workspace — subagents operate in worktrees or they don't operate at all. Violating this rule risks corrupting the working tree, creating merge conflicts, and destroying in-progress work. **If you are a subagent reading this: you MUST be in a worktree. If you are not, STOP and request worktree isolation before proceeding.**

> **🚫 NEVER COMMIT TO MAIN.** Subagents and teammates MUST work on feature branches. Direct commits to `main` are FORBIDDEN. No "quick fix" justifies it. No "it's just one line" justifies it. No urgency justifies it. Create a branch, do the work, submit a PR or merge through the orchestrator. A commit to `main` from a subagent is a catastrophic workflow violation — it bypasses review, pollutes shared history, and can break every other agent's work. **If you are a subagent reading this: if your current branch is `main`, STOP IMMEDIATELY. Create a feature branch before making any commits.**

These two rules are non-negotiable. They exist because violations have real, destructive consequences: corrupted working trees, lost work, broken parallel agent workflows, and unreviewed code landing in production history. Any agent — including the orchestrator — that detects a subagent violating either rule should halt that agent's work immediately.

- **Worktree location**: Always create worktrees under `.claude/worktrees/`, never in the repo root — keeps the project directory clean
- **Worktree dependency bootstrap**: For Node/TypeScript projects, a fresh worktree may not have usable dependency links yet. Before debugging missing modules or broken `tsc`/lint/test resolution, run `pnpm install` in that worktree.
- **Never `rm -rf` a worktree** — always use `git worktree remove` instead. **Never** `git worktree remove` a Claude-managed worktree (`.claude/worktrees/`) — those are cleaned up automatically on session/agent exit.
- **Subagent verification mandate**: Every subagent must run the full test and lint suite before reporting completion. A subagent that reports "done" without passing all checks has not completed its task. The main agent must verify subagent claims — never trust "tests pass" without evidence (test command output or CI results).

## Self-Improvement Loop

See `rules/learning.md` for the full knowledge management system.

- After corrections that reveal a recurring pattern: update memory with the lesson
- Write rules for yourself that prevent the same class of mistake
- Review relevant lessons at session start

## Scope Enforcement at Merge Time

When reviewing a teammate's completed work, diff the branch against main and verify every changed file is in scope. A plan review that rejected a specific step must be enforced at merge — if the out-of-scope files appear anyway, send the teammate back to revert them before merging. Do not accept scope creep because the code is correct.

## Verification Gate

**This is an absolute requirement. No code merges, no task closes, no "done" status without passing this gate.**

Before marking ANY task complete — whether you are a main agent or a subagent:

1. **Run the full test suite** and confirm zero failures. Not "the tests I wrote" — all tests.
2. **Run the linter** and confirm zero warnings.
3. **Run the build** and confirm it succeeds.
4. **Diff behavior** between main and your changes when relevant — confirm no regressions.
5. **Report the actual output** of test/lint/build commands. Stating "tests pass" without running them is a lie, not a shortcut.

If any check fails: fix it, re-run all checks, and only then declare done.

If checks cannot run in the current environment (missing dependencies, no database): this is a blocking problem. Solve it or escalate — do not skip verification.

## Demand Elegance (Balanced)

- If a fix feels hacky, flag it and propose the elegant alternative
- Skip this for simple, obvious fixes — don't over-engineer

## Autonomous Bug Fixing

- When given a bug report with clear reproduction: first write an automated regression test that reproduces the bug and fails against the current code
- Do not implement the bug fix until that failing regression test exists
- After the failing regression test is in place, implement the fix and verify the regression test passes
- Point at logs, errors, failing tests — then resolve them
- Zero context switching required from the user
- Ambiguous scope or multi-system impact: plan first, then fix

## Git Workflow

This project merges directly to `main`, but development commits must be made on feature branches attached to worktrees. Do not commit on `main`. Do not create pull requests or use `gh pr create` unless explicitly instructed.

Always use this sequence — no exceptions:

```bash
git commit                  # commit local work on your feature branch first
git fetch origin
git rebase origin/main      # replay branch commits on top of the latest main
git push --force-with-lease # update the branch after rebasing
git checkout main
git pull --ff-only origin main
git merge --no-ff <feature-branch>   # always create a merge commit
git push origin main
```

Never rebase with staged or unstaged changes — commit first. Never merge stale branch work without first rebasing onto `origin/main`. Never fast-forward a feature branch into `main`: do not use `git merge --ff`, `git merge --ff-only`, or rely on the default fast-forward behavior. Every branch integration into `main` must use `git merge --no-ff` so the merge commit preserves branch history. If the rebase has conflicts, resolve them before pushing or merging.

## Core Principles

- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **No Infrastructure Assumptions**: Never assume a specific infrastructure provider, CI/CD system, container registry, cloud platform, or deployment target unless it is explicitly documented or stated in this project. This includes (but is not limited to) GitHub Actions, GitHub Container Registry, AWS, GCP, Azure, Docker Hub, Vercel, Fly.io, Railway, and any other vendor-specific tooling. If a task requires infrastructure decisions and none are documented, **ask before proceeding**. Do not generate configs, manifests, or code that encodes a provider choice you invented.
- **AskUserQuestion sparingly**: Only for decisions where the wrong choice requires significant rework. For preference questions, pick a sensible default and proceed.
- **After context compaction**: Trust the summary. Do not re-run git status, git diff, git log, or test suites that the pre-compaction portion already completed.

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

## Subagent Strategy

- Use subagents liberally to keep the main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution
- Subagents should generally be isolated in worktrees
- **Worktree location**: Always create worktrees under `.claude/worktrees/`, never in the repo root — keeps the project directory clean
- **Never `rm -rf` a worktree** — always use `git worktree remove` instead. **Never** `git worktree remove` a Claude-managed worktree (`.claude/worktrees/`) — those are cleaned up automatically on session/agent exit.
- **Subagent verification mandate**: Every subagent must run the full test and lint suite before reporting completion. A subagent that reports "done" without passing all checks has not completed its task. The main agent must verify subagent claims — never trust "tests pass" without evidence (test command output or CI results).

## Self-Improvement Loop

- After corrections that reveal a recurring pattern: update memory with the lesson
- Write rules for yourself that prevent the same class of mistake
- Review relevant lessons at session start

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

- When given a bug report with clear reproduction: first write an automated test that reproduces the bug, then fix it
- Point at logs, errors, failing tests — then resolve them
- Zero context switching required from the user
- Ambiguous scope or multi-system impact: plan first, then fix

## Core Principles

- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **AskUserQuestion sparingly**: Only for decisions where the wrong choice requires significant rework. For preference questions, pick a sensible default and proceed.
- **After context compaction**: Trust the summary. Do not re-run git status, git diff, git log, or test suites that the pre-compaction portion already completed.

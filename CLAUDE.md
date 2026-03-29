# This repo is a shared instructions container

`agents` holds shared rules and custom agent definitions consumed by other
projects. It is **not** a software project — there is no application code, no
build system, and no tests.

## What lives here

- `rules/` — Markdown rule files imported into project `CLAUDE.md` files via
  `@` directives
- `agents/` — Agent definition files (`.md` with YAML frontmatter) exposed
  globally via `~/.claude/agents/`

## How consuming projects use this repo

### Submodule setup (preferred)

```bash
# From the consuming project root
git submodule add <this-repo-url> .agents
git submodule update --init --recursive
```

Then import rules in the consuming project's `CLAUDE.md`:

```
@.agents/rules/workflow.md
@.agents/rules/beads.md
# ...add whichever rules apply
```

To update to the latest rules:

```bash
git submodule update --remote .agents
git add .agents
git commit -m "update agents submodule"
```

### Sibling checkout (alternative)

Keep a checkout at `../agents` relative to the consuming project and import:

```
@../agents/rules/workflow.md
```

### Custom agents

To make custom agents available to Claude Code globally:

```bash
ln -sf "$(pwd)/.agents/agents/<agent-name>.md" ~/.claude/agents/
```

Or copy instead of symlink if you want project-local pinning.

## What does NOT live here

- Application code
- Dependencies or package manifests
- CI/CD pipelines
- Project issue tracking state (this repo has no issue tracker)

## Contributing

This repo merges directly to `main`, but development work still happens on a
feature branch in a dedicated worktree. Do not commit on `main`. Do not create
pull requests unless explicitly instructed. This repo has no issue tracker, so
rule and agent changes are tracked with branches and commits.

Always use this sequence:

```bash
git commit                  # commit local work on your feature branch first
git fetch origin
git rebase origin/main      # replay branch commits on top of the latest main
git push --force-with-lease # update the branch after rebasing
git checkout main
git pull --ff-only origin main
git merge --no-ff <feature-branch>
git push origin main
```

Never rebase or pull with staged or unstaged changes. Commit first. Never
fast-forward a feature branch into `main`.

When committing, use the `commit-commands:commit` skill. Do **not** use `commit-commands:commit-push-pr` — this repo has no PRs.

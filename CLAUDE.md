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
- Beads issues (this repo has no issue tracker — changes are committed directly)

## Contributing

Edit rule or agent files directly and commit. No branches, no PRs, no issue
tracking.

Always use this sequence:

```bash
git commit        # commit all local work first (clean tree required for rebase)
git pull --rebase origin main  # rebase on top of any remote changes
git push origin main
```

Never pull with staged or unstaged changes — commit first, then pull, then push.

When committing, use the `commit-commands:commit` skill. Do **not** use `commit-commands:commit-push-pr` — this repo has no PRs.

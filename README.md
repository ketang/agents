# agents

Shared [Claude Code](https://claude.ai/code) rules and agent definitions for use across projects. Rules in `rules/` are imported into project CLAUDE.md files. Agent definitions in `agents/` are symlinked into `~/.claude/agents/` for global availability.

## Usage

In your project's `CLAUDE.md`, import the rules you need:

```markdown
@../agents/rules/workflow.md
@../agents/rules/testing.md
@../agents/rules/go.md
```

This assumes `agents` is checked out as a sibling of your project directory:

```
project/
  agents/          # this repo
  your-project/    # your project
```

## Available Rules

| File | What it covers |
|---|---|
| `rules/workflow.md` | Plan mode, subagents, verification, build-vs-buy, bug fixing |
| `rules/testing.md` | Test standards, coverage targets, test-first bugs, completion checklist |
| `rules/code-quality.md` | Documentation, security basics, no magic numbers, no hardcoded paths |
| `rules/go.md` | Go conventions (error wrapping, slog, context, interfaces, SQL) |
| `rules/react-vite.md` | React / TypeScript / Vite / Mantine conventions |
| `rules/graphql.md` | gqlgen (backend) + gql.tada (frontend) patterns |
| `rules/database.md` | PostgreSQL / pgx / Goose migration patterns |
| `rules/beads.md` | Beads issue tracking, git workflow, plans |
| `rules/learning.md` | Knowledge tracking, error logging, self-maintenance of the knowledge system |

## Available Agents

| File | Model | Purpose |
|---|---|---|
| `agents/planner.md` | Opus | Task decomposition, model tagging, swarm-consumable output |
| `agents/task-coder.md` | Sonnet | Execute well-defined tasks with escalation and output control |

Agent definitions are `.md` files with YAML frontmatter that Claude Code discovers in `~/.claude/agents/`. Symlink or copy individual files there to make them available.

## Setup

Clone this repo alongside your projects:

```bash
cd ~/project  # or wherever your projects live
git clone git@github.com:ketang/agents.git
```

Then add `@import` lines to your project's `CLAUDE.md` for rules, and symlink agent definitions into `~/.claude/agents/`.

# agents

Shared [Claude Code](https://claude.ai/code) rules for use across projects. Each file in `rules/` is a self-contained set of conventions that can be imported independently.

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

## Setup

Clone this repo alongside your projects:

```bash
cd ~/project  # or wherever your projects live
git clone git@github.com:ketang/agents.git
```

Then add `@import` lines to your project's `CLAUDE.md`.

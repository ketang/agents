# agents

Shared [Claude Code](https://claude.ai/code) rules for use across projects. Each file in `rules/` is a self-contained convention set that can be imported independently.

## Usage

Import only the rules your project needs from its local `.agents` checkout:

```markdown
@.agents/rules/workflow.md
@.agents/rules/testing.md
@.agents/rules/go.md
```

Most projects keep this repository as an in-repo git submodule at `.agents/`, which avoids a separate sibling checkout and keeps rule revisions pinned per project.

If a project still uses a sibling checkout, adjust the import paths accordingly.

## Available Rules

| File | What it covers |
|---|---|
| `rules/workflow.md` | Planning, worktree discipline, verification, build-vs-buy, bug fixing |
| `rules/testing.md` | Test standards, coverage targets, test-first bugs, completion checklist |
| `rules/code-quality.md` | Documentation, security basics, no magic numbers, no hardcoded paths |
| `rules/go.md` | Go conventions (error wrapping, slog, context, interfaces, SQL) |
| `rules/react-vite.md` | React / TypeScript / Vite / Mantine conventions |
| `rules/graphql.md` | gqlgen (backend) + gql.tada (frontend) patterns |
| `rules/database.md` | PostgreSQL / pgx / Goose migration patterns |
| `rules/beads.md` | Beads onboarding, issue slicing, git workflow, plans |

## Setup

Recommended project setup:

```bash
git submodule add git@github.com:ketang/agents.git .agents
```

Then import the needed rule files from `.agents/` in the project's `CLAUDE.md` or equivalent agent instruction file.

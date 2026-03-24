# This repo is a document container

`agents` holds shared rules and agent definitions consumed by other projects. It is **not** a software project — there is no application code, no build system, and no tests.

## What lives here

- `rules/` — Markdown rule files imported into project CLAUDE.md files via `@` directives
- `agents/` — Agent definition files (`.md` with YAML frontmatter) symlinked into `~/.claude/agents/`

## What does NOT live here

- Application code
- Dependencies or package manifests
- CI/CD pipelines
- Beads issues (this repo has no issue tracker — changes are committed directly)

## Contributing

Edit rule or agent files directly and commit. No branches, no PRs, no issue tracking. Push to `main`.

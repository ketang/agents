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

- Preferred: add this repo as a `.agents` git submodule and import rules from
  `@.agents/rules/...`
- Optional: keep a sibling checkout and import rules from `@../agents/rules/...`
- Separately, symlink or copy files from `agents/` into `~/.claude/agents/` to
  make custom agents available globally

## What does NOT live here

- Application code
- Dependencies or package manifests
- CI/CD pipelines
- Beads issues (this repo has no issue tracker — changes are committed directly)

## Contributing

Edit rule or agent files directly and commit. No branches, no PRs, no issue
tracking. Push to `main`.

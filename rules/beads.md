# Beads Issue Tracking & Git Workflow

## Onboarding

Run `bd onboard` at session start to load the current project workflow. Keep top-level agent instructions lean and let `bd prime` provide the detailed, up-to-date issue workflow when needed.

## Issue Tracking

```bash
bd ready                               # find unblocked work
bd show <id>                           # inspect issue details and dependencies
bd update <id> --status in_progress    # claim work
bd create "Title" -t bug|feature|task  # create new work
bd close <id>                          # complete work
bd sync                                # sync beads state with git
```

Use `--json` for programmatic invocations.

### Issue Decomposition

Prefer thin end-to-end vertical slices that deliver a complete user or agent outcome. Do not split one behaviour into separate database, API, and frontend issues unless the work is a true standalone prerequisite.

### Agent Work Requires Issues

Any agent doing more than a one-liner, aside from trivial documentation-only edits, should work against a beads issue and use a dedicated feature branch plus worktree. If the current branch is `main`, branch off before making code changes.

### Batch Commands

When inspecting several issues, batch the reads instead of issuing a long sequence of one-off `bd show` commands.

## Git Workflow

Do implementation work on a feature branch in a worktree, not directly on `main`. When work is verified, merge back into `main` from the command line. Only fall back to a pull request when command-line landing is blocked.

Before merging, verify the current branch explicitly:

```bash
git branch --show-current
```

## Implementation Plans

After plan approval, copy the plan from `~/.claude/plans/` into `docs/plans/` with a meaningful name, ideally `<issue-id>-<short-name>.md`. The repo copy is the durable version that survives device changes and later sessions.

## Research Memory

After meaningful architecture or implementation research, save factual findings to project memory proactively and date the entry so stale guidance can be recognized later.

# Beads Issue Tracking & Git Workflow

## Issue Tracking

```bash
bd list                                # all open issues
bd ready                               # issues with no blockers (ready to work)
bd show <id>                           # detailed view with dependencies
bd create --title "..." --description "..."
bd update <id> --status in_progress
bd close <id>
```

### Issue Decomposition

Prefer **thin end-to-end vertical slices** that deliver a complete user or agent outcome. Do not split one behaviour into separate database, API, and frontend issues unless the work is a true standalone prerequisite.

### Agent Work Requires Issues

Any agent doing more than a one-liner (excluding project docs updates) must create a beads issue (`bd create`) and work in a dedicated feature branch + worktree. This keeps work trackable and reversible.

### Batch Commands

**Batch `bd show` calls**: `bd show X && echo --- && bd show Y && echo --- && bd show Z` — never sequential individual calls.

## Git Workflow

When work is complete and verified, merge directly into `main` from the command line. Do not create pull requests unless there is an unresolvable merge problem.

**Before `git merge`**: Always run `git branch --show-current` to verify you are on main. If not, `git checkout main` first.

**No cherry-picking**: Never use `git cherry-pick`. If you need a change from another branch, merge or rebase instead. Cherry-picking creates duplicate commits, breaks bisect, and obscures history.

## Beads in Worktrees

When running `bd` commands from inside a worktree, always target the main repo. Beads state lives in the root working tree, not in the worktree checkout. Run `bd` commands from the main repo directory, or pass the repo root explicitly if the tool supports it:

```bash
# from inside a worktree — change to main repo first
cd /path/to/main-repo
bd list
```

Never create issues, update statuses, or close issues while `cwd` is a worktree — the command may resolve paths incorrectly or write to the wrong location.

## Implementation Plans

After plan mode approval, copy the plan from `~/.claude/plans/` into `docs/plans/` with a meaningful name (e.g., `<project-prefix>-<issue-id>-description.md`). Plans in `~/.claude/` are machine-local; the repo copy syncs across devices.

## Research Memory

After researching codebase architecture or feature implementation status, save factual findings to project memory proactively — don't wait to be asked. Tag entries with date so stale facts can be identified later.

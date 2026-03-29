# Issue Tracking & Git Workflow

This file keeps the historical `beads.md` path for compatibility, but the
policy is tracker-agnostic. Each consuming project must document which issue
tracker it uses and the canonical commands or URLs for that tracker.

## Issue Tracking

```bash
# beads examples
bd list                                # all open issues
bd ready                               # issues with no blockers (ready to work)
bd show <id>                           # detailed view with dependencies
bd create --title "..." --description "..."
bd update <id> --status in_progress
bd close <id>

# GitHub Issues examples
gh issue list
gh issue view <id>
gh issue create --title "..." --body "..."
gh issue edit <id> --add-label in-progress
gh issue close <id>
```

Projects may use Beads, GitHub Issues, or another documented tracker. Agents
must follow the project-local tracker choice instead of assuming `beads`.

### Issue Decomposition

Prefer **thin end-to-end vertical slices** that deliver a complete user or agent outcome. Do not split one behaviour into separate database, API, and frontend issues unless the work is a true standalone prerequisite.

### Agent Work Requires Issues

Any agent doing more than a one-liner (excluding project docs updates) must
link the work to an issue in the project's documented tracker and work in a
dedicated feature branch + worktree. This keeps work trackable and reversible.

### Batch Commands

When reviewing multiple issues, prefer batched or parallel reads over repetitive
single-issue lookups when the tracker tooling supports it.

## Git Workflow

When work is complete and verified, merge directly into `main` from the command line using an explicit merge commit. Do not create pull requests unless there is an unresolvable merge problem.

**Before `git merge`**: Always run `git branch --show-current` to verify you are on main. If not, `git checkout main` first.

**Merge policy**: Always merge completed work with `git merge --no-ff <feature-branch>`. Never fast-forward branch merges into `main`.

**No cherry-picking**: Never use `git cherry-pick`. If you need a change from another branch, merge or rebase instead. Cherry-picking creates duplicate commits, breaks bisect, and obscures history.

## Tracker Commands in Worktrees

If the project uses a tracker with repository-local state, run tracker commands
from the main repo and not from an attached worktree unless the tool explicitly
supports worktrees safely.

```bash
# from inside a worktree, change to the main repo first when required
cd /path/to/main-repo
bd list
```

Never create issues, update statuses, or close issues from a worktree when the
tracker tooling may resolve paths incorrectly or write to the wrong location.

## Implementation Plans

After plan mode approval, copy the plan from `~/.claude/plans/` into
`docs/plans/` with a meaningful name (for example,
`<project-prefix>-<issue-id>-description.md`). Plans in `~/.claude/` are
machine-local; the repo copy syncs across devices.

## Research Memory

After researching codebase architecture or feature implementation status, save factual findings to project memory proactively — don't wait to be asked. Tag entries with date so stale facts can be identified later.

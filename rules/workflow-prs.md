# Pull Request Workflow

## Scope

Import this file together with `rules/workflow-common.md` for consuming
projects that integrate branch work through pull requests.

Each consuming project must document:

- the canonical PR creation command or UI flow
- required reviewers or approval rules
- the allowed merge method after approval
- any branch naming or PR title conventions

## Git Workflow

Development commits must be made on feature branches attached to worktrees. Do
not commit development work on `main`.

Always use this sequence unless the consuming project explicitly documents a
different PR flow:

```bash
git commit                  # commit local work on your feature branch first
git fetch origin
git rebase origin/main      # replay branch commits on top of the latest main
git push --force-with-lease # update the branch after rebasing
gh pr create                # or the project's documented PR creation flow
```

Open the PR only after the branch is rebased onto `origin/main` and passes the
project's verification requirements.

Never rebase with staged or unstaged changes — commit first. Never open or
merge a stale branch without first rebasing onto `origin/main`. If the rebase
has conflicts, resolve them before opening the PR.

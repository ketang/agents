# Direct Merge Workflow

## Scope

Import this file together with `rules/workflow-common.md` for consuming
projects that integrate verified branch work directly into `main` without pull
requests.

Each consuming project must document:

- who is allowed to perform the direct merge
- any additional review or sign-off required before merging
- any branch naming conventions or worktree requirements beyond this baseline

## Git Workflow

Development commits must be made on feature branches attached to worktrees. Do
not commit development work on `main`.

Always use this sequence unless the consuming project explicitly documents a
different direct-merge flow:

```bash
git commit                  # commit local work on your feature branch first
git fetch origin
git rebase origin/main      # replay branch commits on top of the latest main
git push --force-with-lease # update the branch after rebasing
git checkout main
git pull --ff-only origin main
git merge --no-ff <feature-branch>   # always create a merge commit
git push origin main
```

Never rebase with staged or unstaged changes — commit first. Never merge stale
branch work without first rebasing onto `origin/main`.

Before `git merge`, always run `git branch --show-current` to verify you are on
`main`. If not, `git checkout main` first.

Never fast-forward a feature branch into `main`: do not use `git merge --ff`,
`git merge --ff-only`, or rely on the default fast-forward behavior. Every
direct branch integration into `main` must use `git merge --no-ff` so the merge
commit preserves branch history. If the rebase has conflicts, resolve them
before merging.

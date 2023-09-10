---
layout: default
---

# Git command reference

A quick reference for commands that are useful to me. Use at own risk.


## Moving commits between branches

### Moving a specific commit from one branch onto another

```bash
git cherrypick COMMIT_HASH
```

### Moving all commits from one branch to the end of another

```bash
git checkout BRANCH
git pull
git rebase BRANCH_WITH_COMMITS
```

### Moving all commits from one branch to the end of another as a single commit

```bash
git checkout BRANCH_WITH_COMMITS
git rebase -i BRANCH_TO_MERGE_TO
```
This will allow you to interactively edit the commits on your branch that are not on the branch you wish to merge into.
Add “s” for squash next to lines to be squashed. Keep the latest as-is.

If all looks good, you will have a single commit on your branch which can then be merged
```
git checkout BRANCH_TO_MERGE_TO
git pull
git merge BRANCH_WITH_COMMITS
```

### Pulling in the difference between specific files on a branch

This will pull in changes in specific files/folders from a branch as uncommitted changes on the current branch.
Useful if a branch contains some file/folder state that you want to bring through, but you don't want to bring through any specific commit

```bash
git checkout -m BRANCH_WITH_INTENDED_STATE path/to/file.txt
git checkout -m BRANCH_WITH_INTENDED_STATE path/to/folder
```

### Rebase commits from incorrect_branch onto correct_branch

Useful when you have committed changes on a feature branch based on an incorrect branch and want to raise a PR against the correct branch

```bash
git checkout FEATURE_BRANCH
git rebase --onto CORRECT_BASE_BRANCH INCORRECT_BASE_BRANCH
```


## Working with a forked repo

### Adding a fork as a remote upstream repo

This will allow you to interact with the forked repo and is a necessary prerequisite to the commands below referencing `upstream`

```bash
git remote add upstream https://original_repo.git
```

Doing `git remote -v` should now give something like:
```bash
origin https://github.com/USER/REPO (fetch)
origin https://github.com/USER/REPO (push)
upstream https://github.com/UPSTREAM_USER/UPSTREAM_REPO (fetch)
upstream https://github.com/UPSTREAM_USER/UPSTREAM_REPO (push)
```

### Pull upstream changes into your fork

Use this when you have forked a repository and want to pull the upstream changes into your fork

```bash
git fetch upstream
git pull --rebase upstream BRANCH_NAME
```

### Checking out someone’s fork as a branch

This can be useful if you want to work with a branch on someone's fork but don't want to clone their fork separately.

```bash
git remote add THEIR_USERNAME git@github.com:THEIR_USERNAME/THEIR_REPO.git
git fetch THEIR_USERNAME
git checkout -b LOCAL_NAME_FOR_THEIR_BRANCH THEIR_USERNAME/THEIR_BRANCH
```


## Blasting away local changes

### Blowing away uncommitted local changes

```bash
git reset --hard
```

### Blowing away committed (but unpushed) local changes

```bash
git reset --hard origin/BRANCH_NAME
```

### Blowing away committed and pushed changes to your fork

Useful when you have made a mess and want to reset your fork to match the forked repo

```bash
git fetch upstream
git reset --hard upstream/BRANCH_NAME  
git push origin BRANCH_NAME --force
```


## Fixing mistakes

### Revert 4 commits and squash changes into a single reverting commit

```bash
git revert master~4..master
git rebase -i
```

### Undoing a rebase

```bash
git reflog 
git reset --hard HEAD@{5}
```

### Undoing the last local (unpushed) commit

```bash
git reset --soft HEAD~
```


## Working with pull requests

### Checking out a PR as a branch

Useful if you want to grab the PR code locally.
If the pull request is from a fork of your repo, this saves you having to clone that fork

```bash
git fetch origin pull/PR_NUM/head:NEW_LOCAL_BRANCH
git checkout NEW_LOCAL_BRANCH
```


## Miscellaneous

### Find missing commits

Every now and again, something goes missing. This shows changes to the local repository reference logs

```bash
git reflog
```

### Unstage file before committing

```bash
git reset -- FILENAME
```

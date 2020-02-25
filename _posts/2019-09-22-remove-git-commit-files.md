---
categories:
- how-to
- git
comments: true
featured: true
image: images/git-400.jpg
layout: single
sidebar:
  nav: 2019-09-22-remove-git-commit-files
tags:
  - first
title: How to remove files from a pushed git commit

---

Every now and then, I push files to a GitHub repo that I really didn't want to. This includes local config files like `.vscode` or `.ipynb_checkpoints`; I also regularly push files that are too big for GitHub's 2.5 GB limits. This is how to remove files from a pushed git commit.

## TLDR

These are the actual steps, for the impatient.

```console
git reset --soft HEAD~1 # reset to last commit
git restore --staged my_file
git push origin my_branch # --force to rewrite history
```

## The explanation

### git reset,  git revert, and git restore

`git reset` "undoes" previous commits from the repo by setting the git HEAD to an earlier commit. `git revert` creates a new commit. The latter option is what you should use if others have modified the repo since your push.

`git restore` does not update your branch, but is for restoring files in the working tree. 

### git reset soft, hard, and mixed

When to use `--soft`, `--hard`, or `--mixed`? These options choose whether the staging area or work tree is modified, or both.

#### soft

`git reset --soft` doesn't modify the index or the work tree. Your local changes remain in place as changes to be committed.

#### mixed

This is the default option. `git reset` resets the index but not the work tree. Your local changes remain in place, but the differences between your original commit and the reset HEAD will show up as local modifications. 

#### hard

`git reset --hard` modifies both the index and the work tree. This forces your local files to match the commit you've reset to, removing any local changes. 

### Unstaging a file

There are multiple ways to unstage a file with git: `git reset`, `git rm`, and `git restore`. 

`git reset HEAD my_file` unstages local changes to the file, but does not undo them in the working directory. (Note that you can remove local modifications with `git reset --hard my_file`) If this is the first time the file has been added/committed, this has the effect of removing it from the commit.

`git rm --cached my_file` stages the removal of the my_file from the repo, but leaves it in the working directory. 

git now suggests a third option: `git restore --staged my_file`. This will "discard changes in the working directory". This seems to do the same thing as `git reset my_file`.

All three commands do roughly the same thing when the file you wish to delete has just been added. As seen in the TLDR, I use `git restore` because that is the command built for this purpose. `git rm` seems to be the preferred option when the file you wish to delete has already been around for a few commits.

Happy deleting.
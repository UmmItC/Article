---
author: "UmmIt"
title: "Mastering Git: Wayback Commands"
tags: ["Git"]
date: 2023-12-26T04:45:59+08:00
thumbnail: https://cdn.freebiesupply.com/logos/large/2x/git-icon-logo-png-transparent.png 
---

## Overview

Git, the powerful version control system, is an indispensable tool for developers. Understanding its commands and workflows can greatly enhance your efficiency and productivity. In this blog, we'll explore some common and essential Git commands for various scenarios.

## 1. Undoing Commits: `git reset HEAD~`

Imagine making a commit that you want to undo. The `git reset HEAD~` command allows you to reset the current branch's HEAD to the previous commit, effectively `undoing` the last commit. Use this cautiously, especially if you've already pushed changes.

```bash
git reset HEAD~
```

## 2. Force Pushing: `git push origin branch --force`

Force pushing is necessary when you've made changes locally that conflict with the remote repository. This overwrites the remote branch with your local changes. Exercise caution with this command, especially on shared branches.

```bash
git push origin dev --force
```

## 3. Amending Commits: `git commit --amend`

The `--amend` option is used to modify the last commit. It combines staged changes with the previous commit, effectively allowing you to alter the commit message or add changes you forgot.

```bash
git commit --amend
```

This command opens your default text editor, allowing you to modify the commit message or save without changes. It combines staged changes with the previous commit.

## 4. Viewing Commit History

To view a concise history of commits, use:

```shell
git log --oneline
```

This displays a list of commits, showing only the commit hash and commit message.

## 4. Hard Reset: `git reset --hard <commit-hash>`

A hard reset discards changes and moves the HEAD to a specific commit (hash). Use this when you want to discard all changes and move to a previous state.

```bash
git reset --hard c14809fa
```

## 5. Stashing Changes: `git stash`

Stashing is handy when you need to switch branches but have uncommitted changes. It saves your changes in a temporary area, allowing you to switch branches without committing.

```bash
git stash
```

## 6. Stashing Untracked Files: `git stash -u`

The `-u` option with `git stash` stashes untracked files along with changes. This is useful when you want to stash everything, including new files.

```bash
git stash -u
```

## Conclusion

These commands are foundational for effective Git workflows. However, exercise caution when using forceful commands, especially in shared repositories. Always ensure you understand the consequences of each command, and consider creating backups or branches before making significant changes. :DD

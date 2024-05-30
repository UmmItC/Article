---
author: "UmmIt"
title: "How to Edit Git Commit Messages: A Step-by-Step Guide"
tags: ["Git"]
date: 2024-05-30T16:53:03+08:00
---

## Introduction

In the world of version control, mistakes happen – including in commit messages. Fortunately, Git provides a straightforward way to correct those messages, whether it's the first commit or any commit in your repository. Here's a step-by-step guide to editing Git commit messages:

### Step 1: Navigate to Your Repository Directory

Use the `cd` command to navigate to the directory where your Git repository resides. Ensure you're in the correct location to make the changes you need.

### Step 2: Initiate an Interactive Rebase

Start an interactive rebase by entering the following command:

```bash
git rebase -i --root
```

Using `--root` in the command instructs Git to start an interactive rebase from the very first commit in your repository, allowing you to review and edit all commits. If you want to edit specific commits, you can replace `--root` with `HEAD~n`, where `n` is the number of commits you want to edit. For example, to edit the last three commits, you can use `HEAD~3`.

### Step 3: Mark the Commits for Editing

Git will open a text editor displaying a list of commits. For each commit you want to edit, change the word `pick` to `reword` or simply `r` at the beginning of the corresponding line. This indicates that you want to edit the commit message.

### Step 4: Save and Exit

Save your changes and exit the text editor.

Vim: `:wq`
nano: `Ctrl + S and Ctrl +X`

### Step 5: Edit the Commit Messages

Git will pause at each commit marked for editing. For each paused commit, Git will open the text editor, allowing you to modify the commit message. Make your desired changes, then save and exit the text editor.

### Step 6: Complete the Rebase

After editing all desired commit messages, Git will continue with the rebase process, applying your changes.

### Step 7: Force Push Your Changes

You'll need to force push the changes to update the history. Use the following command:

```bash
git push origin master --force
```

### Well done!

With these steps, you can confidently edit Git commit messages whenever needed, ensuring your repository's history remains accurate and well-documented.

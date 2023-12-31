---
title: "Vim: A Quick Guide to Efficient Text Editing"
author: "UmmIt"
tags: ["Vim", "Linux", "Terminal"]
date: 2023-12-22T03:15:00+0800
thumbnail: https://www.blog.tonyswierz.com/wp-content/uploads/2020/09/vim.png
---

## Introduction

vim is a powerful text editor that extends the functionality of Vim, providing an enhanced experience for text editing and coding. If you're new to vim or looking to master its capabilities, this guide will walk you through the fundamental modes, commands, and features.


## Understanding Modes

vim operates in different modes, each serving a specific purpose. Familiarizing yourself with these modes is essential for efficient text editing.

### Normal Mode (Esc)

Normal mode is the default mode where you navigate, manipulate text, and execute commands.

- **Navigation:**
  - `h`, `j`, `k`, `l`: Move left, down, up, and right respectively.
  - `w`, `b`: Move forward or backward by a word.
  - `0`, `$`: Move to the beginning or end of a line.

- **Manipulation:**
  - `x`: Delete the character under the cursor.
  - `dd`: Delete the current line.
  - `yy`: Copy the current line.
  - `p`: Paste after the cursor.
  - `u`: Undo the last action.
  - `Ctrl-r`: Redo.

- **Searching:**
  - `/`: Enter search mode.

### Insert Mode (i)

In insert mode, you can directly type and edit text.

- **Enter Insert Mode:**
  - `i`: Insert before the cursor.
  - `I`: Insert at the beginning of the line.
  - `a`: Insert after the cursor.
  - `A`: Insert at the end of the line.

- **Exit Insert Mode:**
  - `Esc`: Return to normal mode.

### Visual Mode (v)

Visual mode allows you to select and manipulate text.

- **Enter Visual Mode:**
  - `v`: Start character-wise visual mode.
  - `V`: Start line-wise visual mode.

- **Manipulate Selected Text:**
  - `d`: Delete the selected text.
  - `y`: Copy the selected text.
  - `c`: Change the selected text.

## Essential Commands

Learn these essential commands to streamline your vim experience.

- `:%d`: Delete all content.
- `:%y+`: Copy all content.
- `:w`: Save changes.
- `:q`: Quit (close the current file).
- `:wq`: Save and quit.

## Advanced Features

Unlock the full potential of vim with advanced features.

- **Multiple Windows:**
  - `:sp`: Split the window horizontally.
  - `:vsp`: Split the window vertically.
  - `Ctrl-w` + `h`, `j`, `k`, `l`: Navigate between windows.

- **File Navigation:**
  - `:e <filename>`: Open a file.
  - `:b <buffer>`: Switch between buffers.
  - `:ls`: List open buffers.

- **Search and Replace:**
  - `:s/foo/bar/g`: Replace all occurrences of "foo" with "bar" in the current line.
  - `:%s/foo/bar/g`: Replace all occurrences in the entire file.

- **Delete specific text:**
  - `:%s/foo//g`: Deleting all occurrences of "foo" in the entire file.

- **Autocomplete with Plugins:**
  - Install plugins like [coc.nvim](https://github.com/neoclide/coc.nvim) for autocompletion.

## Conclusion

As a Linux user, mastering a terminal-based text editor is essential. If you've become adept at Vim, you're a true Vim hacker—whether it's Vi, Vim, or Neovim! :D

In terms of skills, not many people have mastered Linux, let alone Vim. Although there are text editors like Nano, they lack the robust features of Vim or Neovim, making them less powerful options.

Just like Windows Notepad and Notepad++, Notepad serves as the default text editor on Windows, but it's essentially a basic text editor. It lacks the comprehensive features to function like an integrated development environment (IDE).

So, I highly recommend learning Vi, Vim, Neovim, or Emacs as your daily file editor. Once you master them, you'll experience a significant change. Your computer skills will level up considerably.

Leave GUI programs aside and delve into terminal-based tools to elevate your computer skills and become proficient in the art of terminal hacking.

## Reference

- [Find and Replace Text in Vim](https://linuxhandbook.com/find-replace-vim/)


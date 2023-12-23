---
author: "UmmIt"
title: "Resetting GNOME Settings with a Single Command: A Quick Guide"
description: "Explore this quick guide to learn how to effortlessly reset all your GNOME settings using a simple terminal command—restoring your Linux desktop to its default configurations in no time"
tags: ["GNOME", "Linux"]
date: 2023-11-18T21:34:55+0800
thumbnail: https://www.gnome.org/wp-content/uploads/2023/09/GNOME45-splash.webp
---

## Introduction

Customizing your GNOME desktop environment on Linux can lead to various tweaks and adjustments. However, there may come a time when you want to revert all your GNOME settings to their default configurations. In such cases, the `dconf reset` command is your go-to solution. This quick guide will walk you through the simple steps to reset all your GNOME settings effortlessly.

### Why?

I have previously used Gnome for a long time, but I wanted to explore KDE and use multiple desktop environments with it. However, after installing KDE, I encountered issues with broken fonts which affected the display. I attempted to reset the fonts, but it was unsuccessful. I tried resetting my fonts, but it didn't work. In the end, resetting all the settings on Gnome fixed the issues. Follow these steps to reset the settings.

## Step 1: Open the Terminal

Begin by opening your terminal. You can do this by pressing `Ctrl + Alt + T` or using the application launcher.

## Step 2: Run the Reset Command

Enter the following command in the terminal:

```bash
dconf reset -f /org/gnome/
```

This command will reset all settings under the `/org/gnome/` path.

## Step 3: Confirm the Reset

After executing the command, there won't be any confirmation message. The reset happens instantly. You can now close the terminal.

## Conclusion

Whether you want a fresh start or need to troubleshoot issues related to your GNOME settings, the `dconf reset` command simplifies the process.

---
title: "Fixing Slow Downloading on Steam for Linux: A Quick Guide"
author: "UmmIt"
tags: ["Games", "Steam"]
date: 2024-01-02T03:02:58+0800
---

## Introduction 

Are you tired of sluggish download speeds on Steam while using Linux? Don't worry; there's a solution! In this quick guide, we'll explore steps to fix slow downloading issues on your Steam Linux.

## Editing Steam's Configuration File

1. Open the Steam configuration file using your preferred text editor. Let's use `nvim` for this example:

```shell
nvim ~/.steam/steam/steam_dev.cfg
```

2. Add the following lines to the configuration file:

```shell
@nClientDownloadEnableHTTP2PlatformLinux 0
@fDownloadRateImprovementToAddAnotherConnection 1.0
```

3. Save the changes and exit the text editor.

4. Restart Steam and try download the game again. You should see the Speed has a big change! As shown in the image below:

![Speed](./featured.png)

## How It Works

### 1. HTTP/2 on Linux

The first line, `@nClientDownloadEnableHTTP2PlatformLinux 0`, disables the use of HTTP/2 for downloads on the Linux platform. While HTTP/2 is generally more efficient, some users have reported issues with Steam's implementation on Linux. Disabling it might help fix slow downloading problems.

### 2. Download Rate Improvement

The second line, `@fDownloadRateImprovementToAddAnotherConnection 1.0`, adjusts the download rate improvement by allowing Steam to add another connection. This tweak can be particularly beneficial for users with high-speed internet connections, as it enables Steam to utilize additional connections, potentially resolving slow downloading issues.

The line `@fDownloadRateImprovementToAddAnotherConnection` `1.0` increases the number of connections Steam makes to servers (up to 10, usually connects to around 3, with a hard cap in the code). This adjustment can theoretically improve download speeds.

## References

- [Steam Download Speed is so slow on Linux](https://gist.github.com/FikriRNurhidayat/ce18426ad94fff2140538c0adf0e06ec)
- [Steam Downloads Slow On Linux?! FIX IT](https://iv.datura.network/watch?v=A_kRdad3eb4)
- [Slow steam downloads? Try this!](https://old.reddit.com/r/linux_gaming/comments/16e1l4h/slow_steam_downloads_try_this/)

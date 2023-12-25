---
author: "UmmIt"
title: "FFMPEG Bypass: Download Encrypted HLS Video - Worked most of the HLS video format"
description: ""
tags: ["ffmpeg"]
date: 2023-12-25T05:27:10+0800
thumbnail: ./featured.png
lastmod: 2023-12-25T11:53:02+0800
---

## Introduction

First of all, a very MERRY CHRISTMAS to everyone! 🎄🎁
As we embrace the festive spirit, this article kicks off with a practical guide – a valuable method to master when it comes to downloading videos. No need for third-party tools; we'll walk you through a direct approach using FFMPEG. Let's unwrap the steps!

## Required Tools

Before we dive in, ensure you have the following tools ready:

- `FFMPEG` (for seamless file transfer)
- Terminal (for executing commands)
- Browser (for extracting the essential URL)

### Arch Linux Users: Installing FFMPEG

```shell
sudo pacman -S ffmpeg
```

## Step 1: Uncover M3U8 Link and Copy It

Quick guidance:

1. Open your preferred browser.
2. Press `F12` (Inspect) on your keyboard.
3. Navigate to the 'Network' tab.
4. Head over to the enchanting world of `Jable.tv`.
5. Find the desired video. For example:

   > https://jable.tv/videos/ipx-258-c/

6. Type `m3u8` into 'Filter URLs' to swiftly spot the URL. Once found, copy the M3U8 link.

**Copy and safeguard that magical M3U8 link!**

### Can't Locate the URL?

If the M3U8 link eludes you, it could be a missed step. Try refreshing the URL with F5, replay the video, and you're sure to uncover it. Utilize the 'Filter' feature with the value `m3u8` for efficient searching.

![find](./1.gif)

## Step 2: Spark the Video Download

When you spot `[https @ 000001947ece8180] Opening 'https://qmm-truts.mushroomtrack.com/hls/NFb_sIwTdg3IZ6FeEFo9-g/1656586701/0/108/1082.ts' for reading`, rejoice! The reading is successful, and the download is underway.

```shell
ffmpeg -user_agent "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.182 Safari/537.36" -headers "referer: https://jable.tv/" -i "https://qmm-truts.mushroomtrack.com/hls/NFb_sIwTdg3IZ6FeEFo9-g/1656586701/0/108/108.m3u8" -c copy IPX-258.mp4
```

![find](./downloaded.png)

## Step 3: Download Enchantment - A Few Notes

While the download enchants your screen, you'll witness more files named `IPX-258.mp4`. Don't be fooled; it's still waltzing into your device. Resist the urge to close the terminal midway; otherwise, you might need to restart. The download might conclude at a specific time or when the enchantment feels just right.

### Full Download Demo Code (CAWD-091)

For those seeking a mesmerizing HLS video download, behold the sample code:

```shell
ffmpeg -user_agent "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:92.0) Gecko/20100101 Firefox/92.0" -headers "Referer: https://jable.tv/" -i "https://record-smart.alonestreaming.com/hls/Jyz4bZStuyQzFk6168lSdA/1656587266/8000/8688/8688.m3u8" -c copy CAWD-091.mp4
```

Embark on a journey of seamless video downloads with this magical guide! :DDDD 🎁

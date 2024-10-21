---
author: "UmmIt"
title: "How to install Packet Tracer on Arch Linux with AUR"
description: "Installing Packet Tracer on Arch Linux with AUR, a step-by-step guide."
tags: ["Arch Linux", "Packet Tracer", "AUR"]
date: 2024-10-21T10:38:20+0800
---

# Introduction

Just today, I was trying to install Packet Tracer on my Arch Linux machine. But from the official website, there is no official release for Arch Linux. Only the `.deb` vesrion.

I was looking for a way to install it without any hassle. I found a way to install it using AUR. So, I thought of sharing it with you guys. In this article, I will show you how to install Packet Tracer on Arch Linux with AUR.

Cisco Packet Tracer is a network simulation tool that is used for teaching and learning purposes. Also known as CCNA Course Lab, BUt there's no official release for Arch Linux. We need to download that `.deb` file from the official website, let me show you how to install it on Arch Linux.

>I wont be covering the installation of AUR helper, You should familiar with the AUR helper like `yay` or `paru` :)

## Prerequisites

You need to enroll in the Cisco Networking Academy to download the Packet Tracer. and download that file from the official website.

> https://www.netacad.com/resources/lab-downloads?courseLang=en-US

Choice the Linux version of `.deb` file and download it.

## Step 1: Git clone the AUR repository

First, we need to clone the AUR repository. Open your terminal and run the following command:

```bash
git clone https://aur.archlinux.org/packettracer.git
```

## Step 2: Change the directory

Now, change the directory to the cloned repository:

```bash
cd packettracer
```

## Step 3: Rename the downloaded file

Now, rename the downloaded file to `CiscoPacketTracer822_amd64_signed.deb`. and move it to the cloned repository:.

```bash
mv original_file_name.dev CiscoPacketTracer822_amd64_signed.deb # Rename the file
mv CiscoPacketTracer822_amd64_signed.deb packettracer/ # Move the file to the cloned repository
```

its the step of ensure that the file name is same with the PKGBUILD file.

## Step 4: Install the package

Now, install the package using the following command:

```bash
makepkg -si
```

From the packet tracer, I noticed that its required java runtime environment btw :)

## Finsihed

That's it! You have successfully installed Packet Tracer on Arch Linux using AUR. You can now start using it by searching for it in the application menu.

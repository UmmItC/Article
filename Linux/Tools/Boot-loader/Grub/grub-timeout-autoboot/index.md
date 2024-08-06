---
author: "UmmIt"
title: "How to Set the Default Auto Load Seconds in GRUB"
tags: ["grub", "Linux"]
date: 2024-08-07T07:34:40+0800
---

# Introduction

To set the timeout for auto-boot, follow these steps:

## Step 1: Open the GRUB Configuration File

Open a terminal and run the following command to edit the file with root privileges:

```bash
sudo vim /etc/default/grub
```

## Step 2: Set the Timeout

Adjust the `GRUB_TIMEOUT` to the number of seconds you want GRUB to wait before automatically booting the default entry. For example, to set a 10-second timeout:

```bash
GRUB_TIMEOUT=10
```

## Step 4: Save and Exit

Save the changes by `:wq`

## Step 5: Update GRUB Configuration

After edited the GRUB configuration file, regenerate the GRUB configuration:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Step 6: Reboot

Restart your computer to see the changes take effect.

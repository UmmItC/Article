---
author: "UmmIt"
title: "Setting the Default Kernel Select in GRUB"
tags: ["grub", "GNU/Linux", "Multi-Boot"]
date: 2024-07-31T21:54:00+0800
---

## Overview

If you’re using Multi kernel, or you might thinking the default selected options of that kernel not you want, as my case is the zen of kernel, but i want the default kernel is lts, this is alwasys need to using arrow-key to select the kernel you want. So in this article is gonna to walk you guys want to configure GRUB to boot into a specific kernel by default, follow these steps. This guide will help you set up your system to start with your preferred kernel version automatically.

In general, GRUB already has this feature by setting a variable of `GRUB_DEFAULT`.

### 1. Verify GRUB Entries

Verify the exact menu entries, you can list them using:

```bash
sudo grep menuentry /boot/grub/grub.cfg
```

This command will show all available menu entries, helping you confirm the correct entry name for `GRUB_DEFAULT`.

### 2. Edit the GRUB Configuration File

Open the GRUB configuration file with a text editor:

```bash
sudo vim /etc/default/grub
```

### 3. Set the Default Kernel

In `/etc/default/grub`, you have two options to set the default kernel:

- **By Entry Number**: Set the `GRUB_DEFAULT` variable to the position of the kernel entry in the GRUB menu. For example, to select the second entry:

  ```bash
  GRUB_DEFAULT=1
  ```

- **By Menu Entry Name**: Use the exact menu entry string as it appears in the GRUB menu. For example, with linux-lts:

  ```bash
  GRUB_DEFAULT="Advanced options for Arch Linux>Arch Linux, with Linux linux-lts"
  ```

  Make sure to use the exact string, including spaces and casing.

### 4. Update GRUB Configuration

After edited the GRUB configuration file, regenerate the GRUB configuration:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 5. Reboot

Restart your system to see the result.

### 6. Done

Now, with your grub menu, you should have automatically selected the lts kernel :D

Honestly, grub make everything easy to do :)

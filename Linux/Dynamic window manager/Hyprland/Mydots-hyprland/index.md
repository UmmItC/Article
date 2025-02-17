---
author: "UmmIt"
title: "My Hyprland dotfiles as, all you need to know before using this"
tags: ["GNU/Linux", "Hyprland", "Dynamic window manager"]
date: 2024-08-22T22:50:25+0800
lastmod: 2025-02-17T08:05:10+0800
---

## Thank You for Using My Dotfiles!

First of all, I want to express my gratitude to everyone who has used or checked out my dotfiles on Reddit, YouTube, or elsewhere. This article is intended for users who want to set up their systems like mine, and I’ll cover everything you need to know.

I’ve received several inquiries about how to achieve a similar setup, so I decided to share the process with you.

This article is primarily aimed at newcomers to Hyprland.

![](./featured.png)

## Why This Article?

The README file may sometimes be confusing or unclear. To address this, I’ve created this article to provide detailed explanations and guidelines to help you better understand my dotfiles.

In fact, I don’t frequently update the README file for my dotfiles, but I do regularly update this blog post. So, why not focus on updating this blog post instead?

This approach is similar to many open-source projects, where the README may not always be thoroughly updated. It’s often more beneficial to refer to the blog post or the project’s website for the latest information.

Therefore, I believe that directing you to this article on my blog is a better option. At least, that’s my perspective for now.

## How to Use My Dotfiles

I've created a script to install my dotfiles. You can run the script using the command below:

```shell
bash <(curl -s https://raw.githubusercontent.com/UmmItC/Dotfiles/main/setup.sh)
```

This setup script checks whether `git` and `paru` are installed. If they are not, it will install them, then clone the dotfiles repository and copy the configurations to your system. Alternatively, you can clone my dotfiles and run the `install.sh` script manually:

```shell
git clone --recurse-submodules https://github.com/UmmItC/Dotfiles.git
cd Dotfiles
./install.sh
```

cloning the submodules is included my wallpaper repository, so you can get the defualt wallpaper.

This script will install all the required packages and copy the configurations to your system. The setup is divided into three parts:

1. **Installing Required Packages**: This step installs the necessary packages for my dotfiles.

2. **GPU Packages**: The script will detect your GPU. If you're an AMD user, it will prompt you to install the required packages for your GPU.

3. **Laptop packages**: The script will detect if you're using a laptop. If you are, it will prompt you to install the required packages for your laptop.

4. **Copying Configurations**: This step copies the configurations to your system (located in `~/.config`).

You can choose to skip any of these steps. My script is designed for a modular setup, so you can opt out of installing the required packages or copying the configurations if you prefer.

> **Note**: The packages of installation will use `paru` to compelete the task.

## Fastfetch on Terminal Startup

If you want to display Fastfetch when starting the terminal, add the following line to your `.zshrc` or `.bashrc` file:

```shell
fastfetch --show-errors --pipe false
```

For the Hyprland configuration, you should modify the keybindings in `~/.config/hypr/hyprland/keybinds.conf`. Adjust the values to suit your preferences.

## Post installation

After completing the installation, you should modify the configurations to suit your preferences. Since my dotfiles of the installation are based on my pre-configured settings, you will need to make some adjustments.

### Hyprlock

You will need to modify this file to suit your preferences, this file located at `~/.config/hypr/hyprlock.conf`.

- The monitor name you should set to your monitor name in the `monitor` variable. You can find your monitor name by running `hyprctl monitors`.

### Waybar

You will need to modify this file to suit your preferences, this file located at `~/.config/waybar/config`.

The network interface name should be set to your network interface name in the `interface` variable. You can find your network interface name by running `ip a`.

### Hyprshot

You will need to modify this environment variable to suit your preferences, this file located at `~/.config/hypr/hyprland/env.conf`.

The `HYPRSHOT_DIR` variable should be set to your desired directory. otherwise your screenshots will not function properly.

## Wallpaper

For wallpaper you just need to place the wallpaper file into `~/.wallpaper` directory.

After that, press `ALT + W` to choose the wallpaper you want.


## Showcases

Here are some screenshots of my setup :D

{{< gallery >}}
    <img src="https://dl.ummit.dev/dotfiles-20250217/app-launcher.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/clipboard.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/color-picker.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/neovim.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/neovim-2.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/panel.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/power-management.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/power-management-2.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/upgrade.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/wallpaper-picker.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/windows.png" class="grid-w33" />
    <img src="https://dl.ummit.dev/dotfiles-20250217/windows-2.png" class="grid-w33" />
{{< /gallery >}}

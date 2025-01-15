---
author: "UmmIt"
title: "Arch Linux Fcitx5 Installation Guide for Multilingual Typing"
description: "Learn how to install and configure Fcitx5 on Arch Linux to enable multilingual typing support for languages such as Chinese and Japanese."
tags: ["Arch Linux", "Fcitx", "Input Method", "Multilingual Typing"]
date: 2022-12-17T02:57:50+08:00
lastmod: 2025-01-15T10:09:35+0800
---

## Fcitx5 Introduction

Fcitx5, the successor to Fcitx, is a lightweight input method framework for Linux. Offering additional language support through various addons, it enhances your typing experience on Linux systems. This guide will walk you through the installation process and configuration steps for Fcitx5.

## Install Fcitx5 and Required Addons

Let's start by installing Fcitx5 and the necessary addons to enable multilingual typing support.

```shell
sudo pacman -S fcitx5-chinese-addons libime fcitx5 fcitx5-table-extra
```

### Support Program Windows

To ensure comprehensive language support across various programs, install libraries for technologies used by different applications. Execute the following command:

```shell
sudo pacman -S fcitx5-qt fcitx5-gtk fcitx5-im
```

## Environment variables

Environment variables are necessary to ensure the input method works correctly for x11.

>TIPS: If you under wayland. You can skip this step as well. Wayland dont need to set environment variables for input method.

```shell
sudo vim /etc/environment
```

Paste the following lines:

```shell
export GTK_IM_MODULE=fcitx
export XMODIFIERS=fcitx
export QT_IM_MODULE=fcitx
export SDL_IM_MODULE=fcitx
```

## Configuring Input Methods

You will need to configure the input method to select your desired input language. For this, fcitx5 provides a configuration tool for any Display Manager. Better to use the fcitx5-tool directly.

```shell
sudo pacman -S fcitx5-configtool
```

1. After installed the tool, you should be able to find it in your application menu, open it and configure your input method.
2. Find you preferred input language and add it to the list. (e.g., Quick Classic for 速成輸入法)
3. Use the arrow `<` button to move it to the current input method list.

>TIPS: In case you dont found your preferred input method, You can check the `Only Show Current Language` option to show all available input methods.

![config tool](./configtool.png)

## Restart

After completing the above steps, log out and log back in to apply the changes.

>TIPS: Wayland user don't need to restart the system. The changes will be applied immediately.

## Check System Configuration

Type the following command to verify the system configuration:

```shell
echo $GTK_IM_MODULE
```

If `fcitx5` is displayed, the configuration was successful.

## Font Problem and Solution

If you encounter font display issues, particularly garbled characters, install a Chinese font package. Run the following command:

```shell
sudo pacman -S wqy-zenhei
```

And restart the fcitx5 to apply the changes.

# References

- [Arch Linux - Simplified Chinese Localization](https://wiki.archlinux.org/title/Localization/Simplified_Chinese?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DLocalization_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%2FSimplified_Chinese_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno)
- [ProgrammerAll - Fcitx5 Installation Guide](https://www.programmerall.com/article/6459746231/)
- [YouTube Video Tutorial](https://www.youtube.com/watch?v=yXSDJWtGeKY)

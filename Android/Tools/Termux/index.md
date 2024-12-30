---
author: "UmmIt"
title: "Getting Started with Termux on Android: A Comprehensive Guide"
tags: ["Android", "Termux"]
date: 2023-12-26T05:20:20+0800
thumbnail: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKs2CozWn0jPi6F41w5cxBMwSFmCLunLWxObIG13prOXv1GqXPQr2tVbBXaaob_8R0XJvlLr-TQ23-SzzHGnDVLq7I7NP5jbxYVsf2YwCM0JOgEBK6x_kIKoFdrCM0XC3NnaduX3Yl8sjx719BU6k0YGNvOjIgd5pRaG4Ru9yU1cwFO_GYsbw5NXnM/s16000/termux%20icon.png
lastmod: 2024-12-31T06:15:20+0800
---

## Overview

Termux is a powerful terminal emulator for Android that brings a Linux-like command-line experience to your mobile device. In this guide, we'll walk through the installation process using F-Droid, setting up storage access, and exploring the basic functionality of Termux.

## Installing Termux from F-Droid

To install Termux on your Android device, follow these steps:

1. **Install F-Droid**: F-Droid is an open-source app store that hosts a variety of free and open-source applications, including Termux. You can do this by downloading the F-Droid APK from the [official website](https://f-droid.org/) and installing it on your device. or you can download it from [here](https://f-droid.org/F-Droid.apk).

2. **Search for Termux**: Once F-Droid is installed, open it and search for `Termux` and install it.

3. **Grant Permissions**: After the installation is complete, launch Termux. You will be prompted to grant storage access. Allow them to proceed.

4. **Storage Access**: To access your device's storage from Termux, you need to grant permission. Run the following command in Termux :)

    ```bash
    termux-setup-storage
    ```

    Confirm the permission request when prompted.

## Getting Started with Termux

Now that you have Termux installed and storage access configured, you can start using it as a powerful terminal on your Android device. Here are some tips to help you get started.

### Basic Commands

Termux use the package manager `pkg` or `apt` or `apt-get` to install and manage software packages.

Here are some basic commands to get you started:

- **Update Package List:**
    ```bash
    pkg update
    ```

- **Install a Package:**
    ```bash
    pkg install <package_name>
    ```

- **Accessing Storage**: With `termux-setup-storage`, you have granted access to your device's storage. You can navigate to the storage directory using the `cd` command:

    ```bash
    cd storage/shared
    ls
    ```

- **Full system upgrade**: in termux, we dont need to using sudo to upgrade system.

    ```bash
    apt update -y && apt upgrade -y && apt dist-upgrade -y
    ```

## Conclusion

Actually you can even install a full Linux distribution on your Android device using Termux and launch it using a VNC client. Termux is a versatile tool that can be used for various purposes, from running scripts to setting up a web server. It's a must-have app for anyone who wants to explore the world of command-line interfaces on their Android device.

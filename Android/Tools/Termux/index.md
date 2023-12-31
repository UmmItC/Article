---
author: "UmmIt"
title: "Getting Started with Termux on Android"
tags: ["Android", "Termux"]
date: 2023-12-26T05:20:20+0800
thumbnail: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKs2CozWn0jPi6F41w5cxBMwSFmCLunLWxObIG13prOXv1GqXPQr2tVbBXaaob_8R0XJvlLr-TQ23-SzzHGnDVLq7I7NP5jbxYVsf2YwCM0JOgEBK6x_kIKoFdrCM0XC3NnaduX3Yl8sjx719BU6k0YGNvOjIgd5pRaG4Ru9yU1cwFO_GYsbw5NXnM/s16000/termux%20icon.png
---

## Overview

Termux is a powerful terminal emulator for Android that brings a Linux-like command-line experience to your mobile device. In this guide, we'll walk through the installation process using F-Droid, setting up storage access, and exploring the basic functionality of Termux.

## Installing Termux from F-Droid

To install Termux on your Android device, follow these steps:

1. **Install F-Droid**: F-Droid is an open-source app store for Android. You can download it from [https://f-droid.org](https://f-droid.org) or install it via other app stores like F-Droid, Aurora Store, Neo store and Droid-ify.

>Or you can download the Termux apk only.

2. **Search for Termux**: Once F-Droid is installed, open it and search for "Termux."

3. **Install Termux**: Locate Termux in the search results and tap on it. Then, tap the "Install" button to begin the installation.

4. **Grant Permissions**: After the installation is complete, launch Termux. You will be prompted to grant storage access. Allow this permission by executing the following command:

    ```bash
    termux-setup-storage
    ```

    Confirm the permission request when prompted.

## Getting Started with Termux

Now that you have Termux installed and storage access configured, you can start using it as a powerful terminal on your Android device.

### Basic Commands

Termux uses the Bash shell, and you can use familiar commands like `ls`, `cd`, and `pwd`. The package manager used in Termux is `pkg`, but you can also use `apt` or `apt-get`.

Here are some basic commands to get you started:

- **Update Package List:**
    ```bash
    pkg update
    ```

- **Install a Package:**
    ```bash
    pkg install <package_name>
    ```

- **Navigate to Home Directory:**
    ```bash
    cd ~
    ```

- **List Files:**
    ```bash
    ls
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

Termux turns your Android device into a versatile command-line environment. Whether you want to use it for development, scripting, or general Linux command-line tasks, Termux provides a robust platform. Explore the extensive packages available, and don't forget to check out Termux Wiki and community forums for more tips and tricks. Happy coding!

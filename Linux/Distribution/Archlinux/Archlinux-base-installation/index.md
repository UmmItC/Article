---
author: "UmmIt"
title: "Step-by-Step Guide to Installing Arch Linux from Scratch with KDE Desktop Environment"
description: ""
tags: ["Arch Linux"]
date: 2022-12-17T02:57:50+08:00
thumbnail: http://www.pixelstalk.net/wp-content/uploads/2016/10/Free-Arch-Linux-Image.png
lastmod: 2024-11-07T21:37:12+08:00
---

## Introduction

This comprehensive guide will walk you through the process of installing Arch Linux from start to finish.

> Caiution: Arch Linux in not user-friendly for the beginner of GNU/Linux.

### Before You Begin

GNU/Linux Arch operates on a philosophy of simplicity, user control, and customization. Unlike more user-friendly distributions like Linux Mint or Ubuntu, Arch doesn't provide a graphical user interface (GUI) during the installation process. Instead, it relies heavily on the command line.

If you're new to GNU/Linux or have less than a year of experience, consider gaining more familiarity with GNU/Linux basics before attempting an Arch installation.

### The Command Line is Your Interface

In Arch, every step of the installation process is carried out through the command line. Unlike other distributions that offer GUI-based installers, Arch exposes you directly to the power of the terminal. If you're uncomfortable with the command line or don't understand how commands work, this installation process might be challenging.

### Learning Opportunity

However, Arch isn't just a challenging distribution, it's also a fantastic learning opportunity. With a commitment to simplicity and an extensive official documentation, Arch provides a transparent and educational experience.

like how a GNU/Linux system is built, Arch can be a rewarding journey.

### Official Arch GNU/Linux Wiki

Before starting the installation process, it's highly recommended to familiarize yourself with the [Official Arch GNU/Linux Wiki](https://wiki.archlinux.org/). 
The wiki is an invaluable resource that guides you through the installation process and offers in-depth explanations for various configurations.

### Is Arch Right for You?

In summary, if you're new to GNU/Linux or prefer a more user-friendly experience, you might want to consider other distributions.

However, if you're up for a challenge, eager to deepen your GNU/Linux knowledge, and appreciate a do-it-yourself approach, Arch could be the perfect fit.

# Step-by-Step Guide

This comprehensive guide will walk you through the process of installing Arch Linux from start to finish. Follow the steps in order for a successful installation.

## 1. ISO Download

Download the Arch Linux ISO from the official website: [Arch Linux Download](https://archlinux.org/download/)

## 2. ISO USB Burning

Choose a tool to create a bootable USB from the ISO:

- [balenaEtcher](https://www.balena.io/etcher/) (Mac, Linux, Windows)
- [Rufus](https://rufus.ie/en/) (Windows Only)
- [Pendrivelinux](https://www.pendrivelinux.com/) (Windows only, supports multi-OS)

Alternatively, use `dd` on Linux to create a bootable USB, also known as "burning" the ISO and my recommendation of only using this method on GNU/Linux, no related any software needed, proproly every GNU/Linux distro has `dd` command.

```shell
sudo dd if=/path/to/archlinux.iso of=/dev/sdX status=progress
```

### Optional: Wipe USB Drive

If you want to wipe the USB drive before burning the ISO, use the following command, also have many ways to wipe the USB drive, this is one of the ways. You can use `dd` to write zeros to the USB drive, this will erase all data on the USB drive with random data.

```shell
sudo dd if=/dev/zero of=/dev/sdX bs=4M status=progress
```

and here's the way of wipefs, this command will remove all filesystem signatures from the USB drive.

```shell
sudo wipefs -a /dev/sdX
```

with shred, this command will overwrite the USB drive with random data.

```shell
sudo shred -n 1 -v /dev/sdX
```

## 3. Modify BIOS Settings

Access your BIOS settings and configure them to boot from the USB drive.

Let's say you're trying to boot from the USB drive, you need to access the BIOS settings by pressing the corresponding key during the boot process. The key varies depending on the manufacturer, but common keys include `F2`, `F10`, `F12`, or `Del`. Once you're in the BIOS settings, look for the boot order or boot priority settings and set the USB drive as the first boot device.

and keep remember to turn-off the secure boot option if you're using UEFI mode. Since archlinux doesn't have a signed bootloader.

## 4. Start Arch Linux Live

Boot from the USB drive, and you will be in the Arch Linux live boot.

## 5. Ping Test Network

Now, lets check the network connection with ping command, if got response, that's means the network are connected.

```shell
ping archlinux.org
```

### Network configurations

If you're using a wired connection, the network should be automatically configured. If not, check your LAN port or using wifi (if have) with iwctl.

>The iwctl is part of the iwd package, Just for the note, arch linux already have iwd package installed by default.

```shell
iwctl

[iwd]# device list
[iwd]# station wlan0 scan
[iwd]# station wlan0 get-networks
[iwd]# station wlan0 connect [SSID]
exit

ping archlinux.org
```

## 6. Create Partitions

Identify the disk (e.g., `/dev/sda`) and create four partitions using `cfdisk` or `fdisk`, or `gdisk`, just choose one of them, use your preference.

- 500M for EFI System (Type: EFI System)
- 30G for root (Type: Linux filesystem)
- 25G for home (Type: Linux filesystem)
- 4.5G for swap (Type: Linux swap)

Format the partitions:

```shell
mkfs.fat -F32 /dev/sda1   # Format EFI System partition

mkfs.ext4 /dev/sda2       # Format root partition
mkfs.ext4 /dev/sda3       # Format home partition

mkswap /dev/sda4          # Format swap partition
swapon /dev/sda4          # Enable swap partition
```

Mount the partitions:

```shell
mount /dev/sda2 /mnt      # Mount root partition

mkdir /mnt/home           # Create home directory
mount /dev/sda3 /mnt/home  # Mount home partition
```

The boot partition will be mounted later. we could mount it after the arch-chroot.

## 7. Install Basic Arch System

Install essential packages using `pacstrap`:

```shell
pacstrap -i /mnt base linux linux-headers linux-firmware vim networkmanager dhcpcd pipewire
```

## 8. Generate File System Table (FSTAB)

Generate the file system table, This table is used by the system to determine how to mount the partitions.

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

## 9. Switching new system

Switch to the new system by using `arch-chroot`:

```shell
arch-choot /mnt
```

## 9. Modify Root Password

Set the root password:

```shell
passwd
```

## 10. Add New User & Password

Create a new normal user:

```shell
useradd -m <username>
passwd <username>
```

Add permissions to the new user:

```shell
usermod -aG wheel,storage,power <username>
```

## 11. sudoers.tmp Modification

Edit the `sudoers.tmp` file:

```shell
EDITOR=vim visudo # Default will automatically run to /etc/sudoers.tmp
```

Add the following line below `%wheel ALL=(ALL:ALL) ALL`:

```shell
Defaults timestamp_timeout=0
```

## 12. Set System Language - Modify `locale.gen`

Edit `locale.gen` to set the system language:

```shell
vim /etc/locale.gen
```

Find `en_US.UTF-8 UTF-8` and remove the `#`.

Generate the language:

```shell
locale-gen
```

Create a language config file:

```shell
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
```

## 13. Hostname Settings

Set the hostname:

```shell
echo Archlinux > /etc/hostname
```

Edit the hosts file:

```shell
vim /etc/hosts
```

Add the following lines:

```shell
127.0.0.1 localhost
::1 localhost.localdomain
127.0.0.1 localhost
```

## 14. Timezone Added, Update Hardware Time

Set the timezone and update the hardware clock:

```shell
ln -sf /usr/share/zoneinfo/Asia/Taipai /etc/localtime
hwclock -w
```

## 15. EFI System Install Grub Boot

Install GRUB for UEFI:

```shell
pacman -S grub efibootmgr

mkdir /boot/efi
mount /dev/sda1 /boot/efi

grub-install --target=x86_64-efi --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

## 16. Enable Internet Service

To make sure the network is automatically connected after booted, enable the NetworkManager and dhcpcd services:

```shell
systemctl enable NetworkManager.service
systemctl enable dhcpcd.service
```

## 17. Exit Arch-Chroot

Exit the chroot environment:

```shell
exit
```

## 18. Umount /mnt

Unmount the partitions:

```shell
umount -lR /mnt
```

## 19. Reboot

Reboot the system:

```shell
reboot
```

And now your system should boot into Arch Linux! after that, you can install the desktop environment or window manager. Just choose your preference. For this article, I will install KDE, also using the `sddm` as the display manager and xorg as the backend of the display server.

## 20. Install DE

Install a desktop environment (e.g., KDE):

```shell
sudo pacman -S plasma sddm
```

To ensure our display manager is started at boot, enable the `sddm` service:

```
sudo systemctl enable sddm.service
```

## 21. Reboot

Reboot to complete the installation, first exit the arch system:

```shell
exit
```

Then reboot.

```shell
reboot
```

and now enjoy your Arch Linux with KDE desktop environment :D

## Conclusion

Arch linux is a great distribution for those who want to learn more about Linux and have a more personalized experience. The installation process might be challenging, but it's a rewarding journey that allows you to build a system tailored to your preferences.

After you master the arch linux, you should now feel more comfortable with the command line and have a understanding of how a GNU/Linux system works and also as well as the ability to troubleshoot issues that may arise.

For more hardcores, you can try to install Gentoo or LFS (Linux From Scratch) to further deepen your knowledge of GNU/Linux.

But before that, you should feel arch is simple and easy to use. otherwise, these two distributions will be a nightmare for you. Since they are a source based distribution, you need to compile everything from the source code. and the configuration is more difficult than Arch Linux.

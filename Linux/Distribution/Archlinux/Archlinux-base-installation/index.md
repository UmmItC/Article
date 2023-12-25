---
author: "UmmIt"
title: "Step-by-Step Guide to Installing Arch Linux"
description: ""
tags: ["Arch Linux"]
date: 2022-12-17T02:57:50+08:00
thumbnail: http://www.pixelstalk.net/wp-content/uploads/2016/10/Free-Arch-Linux-Image.png
lastmod: 2023-12-26T12:46:01+08:00
---

## Introduction

This comprehensive guide will walk you through the process of installing Arch Linux from start to finish.

### Before You Begin

**Caution: Arch Linux is Not User-Friendly**

Arch Linux operates on a philosophy of simplicity, user control, and customization. Unlike more user-friendly distributions like Linux Mint, Debian, or Ubuntu, Arch Linux doesn't provide a graphical user interface (GUI) during the installation process. Instead, it relies heavily on the command line. If you're new to Linux or have less than a year of experience, consider gaining more familiarity with Linux basics before attempting an Arch Linux installation.

**The Command Line is Your Interface**

In Arch Linux, every step of the installation process is carried out through the command line. Unlike other distributions that offer GUI-based installers, Arch Linux exposes you directly to the power of the terminal. If you're uncomfortable with the command line or don't understand how commands work, this installation process might be challenging.

**Learning Opportunity**

However, Arch Linux isn't just a challenging distribution; it's also a fantastic learning opportunity. With a commitment to simplicity and an extensive official documentation, Arch Linux provides a transparent and educational experience. If you're ready to delve into the world of Linux and enhance your understanding of how a Linux system is built, Arch Linux can be a rewarding journey.

**Official Arch Linux Wiki**

Before starting the installation process, it's highly recommended to familiarize yourself with the [Official Arch Linux Wiki](https://wiki.archlinux.org/). The wiki is an invaluable resource that guides you through the installation process and offers in-depth explanations for various configurations.

**Is Arch Right for You?**

In summary, if you're new to Linux or prefer a more user-friendly experience, you might want to consider other distributions. However, if you're up for a challenge, eager to deepen your Linux knowledge, and appreciate a do-it-yourself approach, Arch Linux could be the perfect fit.

## Conclusion

**For Arch Linux Veterans: Simplicity Unveiled**

If you've already ventured into the world of Arch Linux, you'll likely find the installation process surprisingly straightforward. Every command-line instruction, once understood, contributes to the simplicity and elegance of the Arch Linux system. What might seem daunting initially transforms into a series of logical and comprehensible steps.

**The Beauty of Understanding Commands**

Arch Linux, with its focus on transparency and simplicity, allows you to grasp the inner workings of your system. Each command you execute contributes to building a customized environment tailored to your preferences. Once you've navigated the learning curve, you'll appreciate the beauty of simplicity in Arch Linux.

**A Personalized Linux Experience**

Arch Linux isn't just an operating system; it's a philosophy. By mastering its installation process, you gain the ability to craft a personalized Linux experience from the ground up. You decide what goes into your system, how it's configured, and which components are essential.

**Continuous Learning Journey**

While the installation might be a one-time process, Arch Linux encourages a continuous learning journey. The Arch community, along with the extensive documentation, serves as a valuable resource as you explore more advanced configurations, package management, and system optimization.

**Celebrate the Mastery**

So, if you've mastered the installation of Arch Linux, take a moment to celebrate your accomplishment. You've not only installed an operating system but also acquired skills that empower you to control and tailor your computing environment.

Remember, Arch Linux is not just an OS; it's a dynamic and rewarding learning experience. Enjoy your journey with Arch!

Now, let's proceed with the installation process!

# Step-by-Step Guide to Installing Arch Linux on a Virtual Machine (VM)

This comprehensive guide will walk you through the process of installing Arch Linux on a virtual machine (VM) from start to finish. Follow the steps in order for a successful installation.

## 1. ISO Download

Download the Arch Linux ISO from the official website: [Arch Linux Download](https://archlinux.org/download/)

## 2. ISO USB Burning

Choose a tool to create a bootable USB from the ISO:

- [balenaEtcher](https://www.balena.io/etcher/) (Mac, Linux, Windows)
- [Rufus](https://rufus.ie/en/) (Windows Only)
- [Pendrivelinux](https://www.pendrivelinux.com/) (Windows only, supports multi-OS)

Alternatively, use `dd` on Linux to create a bootable USB:

```shell
dd if=/path/to/archlinux.iso of=/dev/sdX status=progress
```

## 3. Modify BIOS Settings

Access your VM's BIOS settings and configure them to boot from the USB drive.

## 4. Start Arch Linux Live

Boot the VM from the USB drive, and you will be in the Arch Linux live environment.

## 5. Ping Test Network

Check the network connection with a ping test:

```shell
ping archlinux.org
```

If response, That's means the network are connected. If not, check your LAN port or using wifi (if have) with iwctl.

```shell
iwctl

[iwd]# device list
[iwd]# iwctl station wlan0 scan
[iwd]# iwctl station wlan0 get-networks
[iwd]# station wlan0 connect [SSID]
exit

ping archlinux.org
```

## 6. Create Partitions

Identify the disk (e.g., `/dev/sda`) and create four partitions using `cfdisk`:

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

mkdir /mnt/boot           # Create boot directory
mount /dev/sda1 /mnt/boot  # Mount EFI System partition
```

## 7. Install Basic Arch System

Install essential packages using `pacstrap`:

```shell
pacstrap -i /mnt base linux linux-headers linux-firmware vim networkmanager dhcpcd pulseaudio
```

## 8. Generate File System Table (FSTAB)

Generate the file system table:

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

## 9. Switching new system

```shell
arch-choot /mnt
```

## 9. Modify Root Password

Set the root password:

```shell
arch-chroot /mnt
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
nano /etc/locale.gen
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
nano /etc/hosts
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

mkdir /boot
mount /dev/sda1 /boot

grub-install --target=x86_64-efi --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

## 16. Enable Internet Service

Enable network services:

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

## 20. Install DE

Install a desktop environment (e.g., KDE):

```shell
sudo pacman -S xorg xorg-xinit plasma plasma-desktop plasma-wayland-session kde-applications kdeplasma-addons sddm
```

Enable SDDM service:

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

and now you should see the grub menu!

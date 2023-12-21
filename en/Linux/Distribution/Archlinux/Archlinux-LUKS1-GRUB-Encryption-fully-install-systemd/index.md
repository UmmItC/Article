---
author: "UmmIt"
title: "Full Disk Encryption with GRUB and Including /boot: Step-by-Step Guide"
description: ""
tags: ["LUKS", "Linux", "Arch linux"]
date: 2023-12-21T11:42:02+0800
thumbnail: https://battlepenguin.com/images/tech/alpine/grub-luks-unlock.jpg
---

## Introduction

Since systemd-boot doesn't support encrypted /boot, grub does. There are not so good points though, like only luks1 and argon2id are not supported. However, in this short guide I will teach you how to encrypt your /boot to be fully encrypted with our disk.

### Step 1: Encrypt the Disk

To begin, encrypt your disk using the LUKS (Linux Unified Key Setup) format. However, note that GRUB only supports LUKS1, so avoid using certain options:

```shell
cryptsetup luksFormat --type luks1 --cipher aes-xts-plain64 --hash sha256 --iter-time 10000 --key-size 256 --pbkdf argon2id --use-urandom --verify-passphrase /dev/sda2
```

Ensure you answer "YES" when prompted. GRUB doesn't support the `--pbkdf argon2id` option, so it's crucial to stick to LUKS1 for compatibility.

### Step 2: Open LUKS Device and Set Up Logical Volumes

After formatting, open the LUKS device and set up logical volumes using LVM (Logical Volume Manager):

```shell
cryptsetup open /dev/sda2 crypt
pvcreate /dev/mapper/crypt
vgcreate vol /dev/mapper/crypt
lvcreate -l 3%FREE vol -n swap
lvcreate -l 50%FREE vol -n root
lvcreate -l 100%FREE vol -n home
```

Format the root and home volumes:

```shell
mkfs.btrfs /dev/vol/root
mkfs.btrfs /dev/vol/home
```

Create swap space:

```shell
mkswap /dev/vol/swap
swapon /dev/vol/swap
```

Mount the volumes:

```shell
mount /dev/vol/root /mnt
mkdir /mnt/home
mount /dev/vol/home /mnt/home
```

### Step 3: Prepare for GRUB Installation

Since GRUB supports EFI systems, mount the EFI system partition:

```shell
mount /dev/sda1 --mkdir /mnt/boot/efi
```

Now, proceed with the essential package installations:

```shell
pacstrap -i /mnt base base-devel linux linux-firmware linux-headers lvm2 nano dhcpcd networkmanager pipewire
```

Generate the `/etc/fstab` file:

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

### Step 4: Configure mkinitcpio.conf

Edit the `/etc/mkinitcpio.conf` file, ensuring that the `HOOKS` line includes `lvm2` and `encrypt`. It should look like this:

```shell
HOOKS=(base udev autodetect modconf kms keyboard keymap consolefont block lvm2 encrypt filesystems fsck)
```

Save the changes and regenerate the configuration:

```shell
mkinitcpio -P
```

### Step 5: Install and Configure GRUB

Install GRUB and efibootmgr:

```shell
sudo pacman -S grub efibootmgr
```

Configure the GRUB file:

```shell
sudo nvim /etc/default/grub
```

Edit `GRUB_CMDLINE_LINUX_DEFAULT`:

```shell
cryptdevice=/dev/nvme0n1p2:cryptlvm root=/dev/mapper/vol-root
```

Set `GRUB_ENABLE_CRYPTODISK` to "y".

Install GRUB:

```shell
grub-install --recheck /dev/sda1
```

Generate the GRUB configuration:

```shell
grub-mkconfig -o /boot/grub/grub.cfg
```

### Step 6: Reboot and Decrypt

Reboot your system. You'll notice that GRUB prompts you to enter the passphrase or password for decryption. After successfully decrypting, you'll encounter another decryption prompt for your volume disk.

>Note: The decryption process may take some time, and entering the wrong passphrase will lead to a GRUB recovery mode. Reboot and try again.

## Conclusion

Congratulations! Your system is now fully encrypted with GRUB, providing enhanced security for your Arch Linux installation.

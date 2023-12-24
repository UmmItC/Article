---
author: "UmmIt"
title: "Full Disk Encryption with GRUB and Including /boot: Step-by-Step Guide"
description: ""
tags: ["LUKS", "Linux", "Arch linux"]
date: 2023-12-21T11:42:02+0800
thumbnail: https://battlepenguin.com/images/tech/alpine/grub-luks-unlock.jpg
lastmod: 2023-12-22T12:28:23+0800
---

## Introduction

Since systemd-boot doesn't support encrypted `/boot`, grub does. There are not so good points though, like only luks1 and argon2id are not supported. However, in this short guide I will teach you how to encrypt your /boot to be fully encrypted with our disk.

### Step 1: Encrypt the Disk

To begin, encrypt your disk using the LUKS format. However, note that GRUB only supports LUKS1, so avoid using certain options:

```shell
cryptsetup luksFormat --type luks1 --cipher aes-xts-plain64 --hash sha256 --iter-time 10000 --key-size 256 --use-urandom --verify-passphrase /dev/sda2
```

Ensure you answer `YES` when prompted. GRUB doesn't support the `--pbkdf argon2id` option, so it's crucial to stick to LUKS1 for compatibility.

### Step 2: Open LUKS Device and Set Up Logical Volumes

After formatting, open the LUKS device and set up logical volumes using LVM (Logical Volume Manager):

```shell
cryptsetup open /dev/sda2 crypt # Decrypting disk and create mapper named 'crypt'
pvcreate /dev/mapper/crypt # Create physical volume named 'crypt'
vgcreate vol /dev/mapper/crypt # Create volume group named 'vol'
lvcreate -l 3%FREE vol -n swap # Create logcial volume and set this size uses 3% of this partition and named to swap.
lvcreate -l 50%FREE vol -n root # Create logcial volume and set this size uses 50% of this partition and named to root.
lvcreate -l 100%FREE vol -n home # Create logcial volume and set this size uses 100% of this partition and named to home.
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
pacstrap -i /mnt base base-devel linux linux-firmware linux-headers lvm2 neovim dhcpcd networkmanager pipewire
```

Generate the `/etc/fstab` file:

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

The process of installing Arch Linux is the same as that of ArchLinux. If you are unfamiliar with the process, please refer to this article: [Complete Guide to Installing Arch Linux with LUKS Encryption and GNOME Desktop Environment](/posts/en/linux/distribution/archlinux/archlinux-luks-encryption-fully-install-systemd/).

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
pacman -S grub efibootmgr
```

Configure the GRUB file:

```shell
nvim /etc/default/grub
```

Edit `GRUB_CMDLINE_LINUX_DEFAULT`:

```shell
cryptdevice=/dev/nvme0n1p2:crypt root=/dev/mapper/vol-root
```

>Note: Do not use a UUID, the disc may not be found.

and set `GRUB_ENABLE_CRYPTODISK` to "y".

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

>Note: The decryption process may take some time, and entering the wrong passphrase will lead to a GRUB rescue mode. you need to reboot and try again.

## Conclusion

Congratulations! Your system is now fully encrypted with GRUB, providing enhanced security for your Arch Linux installation.

## References

- [Alpine Linux with Full Disk Encryption](https://battlepenguin.com/tech/alpine-linux-with-full-disk-encryption/)
- [[SOLVED] Grub with lvm on LUKS -> rescue mode. Incomplete grub.cfg?](https://bbs.archlinux.org/viewtopic.php?id=246628)
- [Arch Linux | LVM on LUKS with GRUB | Encrypted Installation](https://invidious.einfachzocken.eu/watch?v=FcNQQxtPA0A)
- [Encrypted /boot](https://wiki.archlinux.org/title/GRUB#Encrypted_/boot)
- [dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system)

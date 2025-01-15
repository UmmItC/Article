---
author: "UmmIt"
title: "Complete Guide to setting up LUKS on LVM encryption in Arch Linux (Minimal System)"
description: "A detailed guide on setting up LUKS (Linux Unified Key Setup) encryption on LVM. with Minimal system installation."
tags: ["Arch Linux", "LUKS", "LVM", "Encryption"]
date: 2023-08-10T02:08:10+0800
thumbnail: https://i.ytimg.com/vi/KYwdUKYQjII/maxresdefault.jpg
lastmod: 2025-01-15T08:18:15+0800
---

## Setting Up LUKS Encryption

Welcome to a detailed guide on setting up LUKS (Linux Unified Key Setup) encryption (LVM) as part of your Arch Linux installation process. LUKS encryption provides an additional layer of security for your data.

## Enabling Time Synchronization

Before delving into encryption, it's essential to ensure accurate timekeeping on your system. Enabling Network Time Protocol (NTP) synchronization will synchronize your system's clock with remote servers.

```shell
timedatectl set-ntp true
```

## Partition Setup

Before installing Arch Linux, you need to partition your disk. You can use tools like `cfdisk`, `fdisk`, or `gdisk` for this purpose. In this guide, we will use `gdisk`.

The parition layout will be using LVM, so there only contains 2 partitions.

First off, you need to know which storage devices you want to partition and that name of the device. You can list the available storage devices using the following command:

```shell
lsblk
```

2. Initialize and Create Partitions: Utilize the `gdisk` tool to initialize your disk and create the necessary partitions:

```shell
gdisk /dev/sda
```

   While inside the `gdisk` interface, execute the following actions:

   - Type `o` to create a new GPT partition table and confirm with `y`.
   - Create the EFI boot partition by typing `n`, followed by `Enter` first sector, `+1G` for the last sector, and `ef00` for the partition type code.
   - Create the LVM with LUKS partition by typing `n`, followed by `Enter` until the type code prompt, then `8e00` for the partition type code.
   - Save your modifications with `w` and confirm the changes by typing `y`.

Your disk should now have the following partitions:

| Size   | Type                | Code  |
|--------|---------------------|-------|
| 1G     | EFI System          | ef00  |
| 100G   | Linux LVM           | 8e00  |

## Format Boot Partition

Our partition layout is ready, now we need to format the partitions. We will start by formatting the EFI boot partition. formatted as FAT32. This filesystem format aligns with UEFI booting requirements.

```shell
mkfs.fat -F32 /dev/sda1
```

## Encrypt Your Partition (LUKS)

Configure encryption using LUKS (Linux Unified Key Setup) and create logical volumes to efficiently manage your filesystem.

```shell
cryptsetup luksFormat --type luks2 --cipher aes-xts-plain64 --hash sha256 --iter-time 10000 --key-size 256 --pbkdf argon2id --use-urandom --verify-passphrase /dev/sda2

YES
```

   This command initiates the LUKS encryption process with specific parameters:
   
   - `--type luks2`: Selects LUKS version 2 for encryption.
   - `--cipher aes-xts-plain64`: Chooses the AES encryption algorithm in XTS mode.
   - `--hash sha256`: Specifies SHA-256 as the hash function.
   - `--iter-time 10000`: Sets the time for key derivation iterations.
   - `--key-size 256`: Determines the key size in bits.
   - `--pbkdf argon2id`: Selects the Argon2id key derivation function.
   - `--use-urandom`: Utilizes the `/dev/urandom` entropy source.
   - `--verify-passphrase`: Ensures you verify your chosen passphrase.

   Follow the prompts and input your chosen passphrase when prompted. Remember that this passphrase will be required to unlock and access the encrypted partition.

## Open the Encrypted Partition

LUKS partition is encrypted, for open it:

```shell
cryptsetup open /dev/sda2 crypt
```

This command opens the encrypted partition and maps it to the `crypt` device. and have a new mapper named as `crypt`.

## Create Physical and Logical Volumes

Next, you'll create physical and logical volumes to manage your filesystem efficiently. These volumes will serve as the foundation for your Arch Linux installation.

Create a physical volume:

```shell
pvcreate /dev/mapper/crypt
```

Create a volume group named `vol`:

```shell
vgcreate vol /dev/mapper/crypt
```

Create logical volumes for swap, root, and home directories:

```shell
lvcreate -L 12G vol -n swap
lvcreate -l 50%FREE vol -n root
lvcreate -l 100%FREE vol -n home
```

These commands create logical volumes for the swap, root, and home directories, allocating the desired percentage of space for each volume. For the 

## Format and Mount Partitions

In this step, we will format and mount the partitions necessary for the Arch Linux installation. Properly configuring these partitions is crucial for ensuring a stable and functional system. We'll cover formatting the `root` and `home` volumes, as well as creating and enabling swap space. Let's delve into the details:

### Root and Home

We'll need to format the `root` and `home` volumes with btrfs filesystem. To format the `root` and `home` volumes with the Btrfs filesystem, execute the following commands:

```shell
mkfs.btrfs /dev/vol/root && mkfs.btrfs /dev/vol/home
```

### Swap

Swap space is an integral part of your system's memory management. It provides additional virtual memory when physical RAM is fully utilized. Creating and enabling swap space ensures that your system can handle memory-intensive tasks without performance degradation.

To create and enable swap space on the designated `swap` logical volume, use these commands:

```shell
mkswap /dev/vol/swap && swapon /dev/vol/swap
```

### Mount Root and Home Partitions

Time to mount the `root` and `home` partitions to the `/mnt` directory. Execute the following commands:

```shell
mount /dev/vol/root /mnt
```

```shell
mount /dev/vol/home --mkdir /mnt/home
```

>The EFI partition will not be mounted here, we'll mount it after chrooting into the new system.

## Install Essential Packages

This is the time to install essential packages that form the core of your Arch Linux system. These packages provide foundational tools and utilities that enable system management, software development, and hardware compatibility.

```shell
pacstrap -i /mnt base base-devel linux linux-firmware linux-headers lvm2 vim networkmanager sudo
```

## Automate Mounts with the `fstab` File

To ensure that your filesystems are automatically mounted during system boot, you need to generate the `/etc/fstab` file. This file contains information about your partitions and their mount points, enabling the system to mount them correctly.

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

## Chroot into the New System

To enter this new system environment, type the following command:

```shell
arch-chroot /mnt
```

## Initialize the Pacman Keyring

Better to refresh the pacman keyring, execute the following commands:

```shell
pacman-key --init && pacman-key --populate archlinux
```

## Enable Network Services

Enable the NetworkManager service to manage network connections.

```shell
systemctl enable NetworkManager
```

## Set the System Locale

Configuring the system locale is an essential task to ensure proper language support and effective localization within your Arch Linux environment. The system locale defines the language, character encoding, and other regional settings that your system will use.

Execute the following command to open the `locale.gen` file in the `vim` text editor:

```shell
vim /etc/locale.gen
```

3. Inside the text editor, navigate to the line that corresponds to your desired locale. For instance, to enable the English (United States) locale, find the line containing `en_US` and remove the `#` symbol at the beginning of the line.

4. Save the file and exit the text editor.

5. Generate the selected locale by running the command:

```shell
locale-gen
```

This command generates the necessary locale files based on your configuration.

6. Set the system's default locale by entering the following command:

```shell
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

## Set User Passwords

User account management is a crucial aspect of system security. Follow these steps to establish secure passwords for both the root user and a new user:

1. Set the root password by entering the following command and following the prompts:

```shell
passwd
```

2. Create a new user account using the `useradd` command. Replace `username` with the desired username:

```shell
useradd -m username
```

3. Set the password for the newly created user by running the following command and following the prompts:

```shell
passwd username
```

## Basic group allocation

To ensure that your user account has the necessary permissions to perform system tasks, allocate the user to essential groups.

```shell
usermod -aG wheel,storage,power username
```

## Configure sudoers file

sudo aren't allowed by default, you need to enable it by editing the sudoers file.

```shell
EDITOR=vim visudo
```

3. Uncomment the line `%wheel ALL=(ALL) ALL` by removing the `#` symbol at the beginning of the line.

### Timestamp Timeout

To avoid the entering password delay every time, you can set the timestamp timeout to 0.

```shell
Defaults timestamp_timeout=0
```

## Set Hostname

Assigning a hostname to your Arch Linux system is essential for network identification. As example, we will set the hostname to `arch`.

```shell
echo arch > /etc/hostname
```

## Set Hosts File

To associate the hostname with the loopback address, modify the `/etc/hosts` file, adding the following line:

```shell
127.0.0.1 localhost
::1       localhost
127.0.0.1 arch.localdomain  localhost
```

## Set Timezone

Configuring the correct timezone ensures accurate timekeeping on your Arch Linux system. to do this, create a symbolic link to the appropriate timezone file and synchronize the hardware clock with the system time. As example, we will set the timezone to `Asia/Taipei`.

```shell
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```

Synchronize the hardware clock with the system time:

```shell
hwclock --systohc
```

## Configure mkinitcpio

You need to configure `mkinitcpio` to include necessary modules for LVM2 and encryption support. This step ensures your encrypted partitions can be properly accessed during the boot process. Edit the `/etc/mkinitcpio.conf` file:

1. Locate the `HOOKS` line and add `lvm2` and `encrypt` to the list of hooks. Your modified line should look like this:

   ```shell
   HOOKS=(base udev autodetect modconf kms keyboard keymap consolefont block lvm2 encrypt filesystems fsck)
   ```

3. Save the file and exit the text editor.

4. Regenerate the initramfs with the updated configuration:

   ```shell
   mkinitcpio -p linux
   ```

## Format and Mount EFI Partition

It's time to format the EFI partition and mount it to the `/boot/efi` directory. Execute the following commands:

```
mkfs.fat -F32 /dev/sda1
```

and mount it:

```shell
mount /dev/sda1 --mkdir /boot/efi
```

## Install and Configure Bootctl

Now, we'll configure the systemd-boot bootloader to manage the boot process for your Arch Linux system.

1. Install `bootctl` to the `/boot/efi` directory:

   ```shell
   bootctl --path=/boot/efi install
   ```

2. Open the `loader.conf` file for editing using the `vim` text editor:

   ```shell
   vim /boot/loader/loader.conf
   ```

3. Inside the text editor, add the following lines to set the default boot options:

   ```shell
   default arch
   timeout 10
   editor 0
   ```

4. Save the file and exit the text editor.

5. Create a boot entry for your Arch Linux installation in the bootloader configuration. This ensures that you can easily select Arch Linux during boot.

   ```shell
   vim /boot/loader/entries/arch.conf
   ```

6. Inside the text editor, add the following lines to specify the Linux kernel and initramfs files:

   ```shell
   title Arch linux
   linux /vmlinuz-linux
   initrd /initramfs-linux.img
   ```

7. Save the file and exit the text editor.

### Add Encryption Options

To ensure that your encrypted partition is properly decrypted during the boot process, you need to add encryption options to the bootloader configuration. This step is crucial for seamless decryption and access to your encrypted root volume.

1. Add the UUID of the encrypted partition to the bootloader configuration. First, obtain the UUID of the encrypted partition using the `blkid` command:

    ```shell
    blkid /dev/sda2 >> /boot/loader/entries/arch.conf
    ```

2. Now, reopen the `arch.conf` file for further editing:

   ```shell
   vim /boot/loader/entries/arch.conf
   ```

3. Refine the following lines within the file to precisely outline the encryption options, while replacing `<UUID>` with the actual UUID obtained in the previous step:

   ```shell
   options cryptdevice=UUID=<UUID>:cryptlvm root=/dev/vol/root quiet rw
   ```

4. Save the file and exit the text editor.

#### Finalize Boot Configuration

And this is the sample of the `arch.conf` file, you should similar to this, if you done correctly.

```shell
title Arch linux
linux /vmlinuz-linux
initrd /initramfs-linux.img

options cryptdevice=UUID=<UUID>:cryptlvm root=/dev/vol/root quiet rw
```

## Finish Installation

After completing these steps,  Now exit your current user then umount your arch system. You can enjoy your new Arch Linux system with LUKS encryption. (But no GUI XD)

```shell
exit
umount -R /mnt
```

you can proceed to reboot your system.

```shell
reboot
```

## References

- [No mkinitcpio preset present](https://unix.stackexchange.com/questions/571124/no-mkinitcpio-preset-present)
- [Installation guide](https://wiki.archlinux.org/title/Installation_guide)
- [How to Dual Boot Arch Linux and Windows 11/10](https://onion.tube/watch?v=JRdYSGh-g3s)
- [Como instalar ArchLinux com UEFI criptografia LUKS](https://onion.tube/watch?v=K4pcd0B_eGk)
- [[1d] | Arch Linux Base Install on UEFI with LUKS Encryption](https://onion.tube/watch?v=XNJ4oKla8B0)
- [Create LUKS encrypted root and efi boot partitions for Arch Linux](https://onion.tube/watch?v=cxMYR617a5E)

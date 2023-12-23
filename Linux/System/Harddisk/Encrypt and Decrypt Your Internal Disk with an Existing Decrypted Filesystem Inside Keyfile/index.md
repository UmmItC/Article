---
author: "UmmIt"
title: "Encrypt and Decrypt Your Internal Disk with an Existing Decrypted Filesystem Inside Keyfile"
description: ""
tags: ["LUKS"]
date: 2023-12-08T10:10:02+0800
---

## Introduction

In the previous article, I demonstrated how to auto-mount an internal disk during system boot-up. In this article, I'll guide you through the process of encrypting your internal disk using LUKS. This method involves decrypting your first disk, locating the keyfile on that disk, and using it to decrypt subsequent disks. Thus, you'll need to decrypt the first disk to decrypt your internal disk. This ensures that the keyfile is stored on your first disk, preventing them from being separated.

If you're unfamiliar with fully encrypting your entire disk, refer to this article:

>[Complete Guide to Installing Arch Linux with LUKS Encryption and GNOME Desktop Environment ](/posts/en/linux/distribution/archlinux/archlinux-luks-encryption-fully-install-systemd/).

In summary, this method uses both a passphrase and a keyfile for disk decryption. The keyfile is stored on your system.

### Compatibility

Before proceeding, ensure you've backed up all data on your internal disk, as the upcoming command will overwrite all data. Also, verify compatibility:

- The target disk is already fully encrypted.
- You want to encrypt an internal disk.

### What It Does

- Encrypts by passphrase and keyfile.
- Requires both passphrase and keyfile for decryption.
- Allows decryption using the keyfile.

## Step 1: Switch to Root User

Since all operations require root permissions, switch to the root user to expedite the process.

```shell
su
```

## Step 2: Identify Your Disk

To check which disk you're targeting for encryption, enter the following command:

```shell
lsblk
```

## Step 3: Create File System

Linux requires a GPT table for disks. Use `gdisk` to create the GPT table and format the file system. After marking the target disk, process the `gdisk` for this disk:

```shell
gdisk /dev/sda
o
y
n
enter
enter
enter
enter
p (ensure the file system is correct)
w
y
```

After this process, a file system will be created on `/dev/sda1`.

## Step 4: CryptLUKS Setup

Now, let's encrypt the disk using `cryptsetup`:

```shell
cryptsetup luksFormat --type luks2 --cipher aes-xts-plain64 --hash sha256 --iter-time 10000 --key-size 256 --pbkdf argon2id --use-urandom --verify-passphrase /dev/sda1

YES
```

## Step 5: Decrypt LUKS

This command will encrypt the disk and map it to a name, such as `4tbhdd`.

```shell
cryptsetup open /dev/sda1 4tbhdd
```

When using the cryptsetup luksOpen command to unlock the LUKS encryption device, the `/dev/mapper/` name assigned to the mapped device is determined by the `open` command. and the name can be changed as needed.

## Step 6: Create File System

As we have created a new mapper for the encrypted disk (`4tbhdd`), we also need to create a file system on this mapper. Execute the following command to accomplish this:

```shell
mkfs.btrfs /dev/mapper/4tbhdd
```

This command initializes a Btrfs file system on the specified mapper, ensuring that the encrypted disk is ready for use.

## Step 7: Create Keyfile

Now, create a keyfile that will be imported into your previously decrypted disk, enhancing the security of your setup.

```shell
dd if=/dev/urandom of=/root/keyfile bs=10000 count=819
```

## Step 8: Set the Permission of Keyfile

Set the permissions for the keyfile to ensure only the root user can access it, enhancing security.

```shell
chmod 0400 /root/keyfile
```

## Step 9: Add Keyfile to LUKS Device

Add the keyfile to the LUKS device to reinforce security and enable decryption.

```shell
cryptsetup luksAddKey /dev/sda1 /root/keyfile
```

## Step 10: Get the UUID of LUKS

Retrieve the UUID of the LUKS device for the next steps by running the following command:

```shell
cryptsetup luksUUID /dev/sda1
```

### Add to crypttab

Add the UUID and related content to the `crypttab`. This file is crucial for loading the key during the system boot-up without requiring a passphrase.

```shell
nvim /etc/crypttab

4tbhdd          /dev/disk/by-uuid/<uuid-of-luks>  /root/keyfile   luks
```

The `crypttab` entry you provided essentially automates the process of unlocking and mapping the LUKS-encrypted device during the system boot. The entry specifies the necessary details, such as the device path, the keyfile, and the desired mapping name (`4tbhdd`). This way, you don't need to manually run the `cryptsetup luksOpen` command during each boot.

Here's the breakdown:

- The system reads the `/etc/crypttab` file during the boot process.
- It identifies the `4tbhdd` entry.
- It automatically runs a command similar to this:

```bash
cryptsetup luksOpen /dev/disk/by-uuid/<UUID> 4tbhdd --key-file /root/keyfile
```

- The LUKS-encrypted device is then mapped to `/dev/mapper/4tbhdd`, and you can access it using this mapped device name.

This automation is convenient for managing encrypted devices, especially when dealing with multiple LUKS-encrypted partitions or disks. It simplifies the process of unlocking and mapping during system startup.

## Step 12: Get the UUID of mapper

Copy the UUID of the mapper for further use:

```shell
blkid /dev/mapper/4tbhdd >> /etc/fstab
```

### Add to fstab

Edit the `/etc/fstab` file and add an entry for mounting the mapper during system boot-up:

```shell
nvim /etc/fstab

# /dev/mapper/4tbhdd 4TB HDD
UUID=<UUID-of-mapper>  /mnt/4tbhdd     btrfs       defaults        0 0
```

Explanation:

- The `blkid` command is used to retrieve the UUID of the mapper (`4tbhdd`).
- The obtained UUID is then appended to the `/etc/fstab` file, ensuring that the mapper is mounted automatically during the system boot-up.


## Step 14: Create Mounting Point

Since we are targeting the mount point `/mnt/4tbhdd`, let's create this directory using the following command:

```shell
mkdir /mnt/4tbhdd
```

This directory will serve as the mount point for our encrypted internal disk.

## Step 15: Adjusting Permissions

Because the permissions are set to root only, use `chown` to allow a regular user to perform any action.

```shell
chown user:group /mnt/4tbhdd/
```

## Step 16: Reboot

Now, reboot your system. After the reboot, you should notice that you are not required to enter any passphrase manually. Use the `lsblk` command to verify that your encrypted internal disk has been successfully mounted:

```shell
lsblk
NAME           MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda              8:0    0   3.6T  0 disk  
└─sda1           8:1    0   3.6T  0 part  
  └─4tbhdd     254:4    0   3.6T  0 crypt /mnt/4tbhdd
```

Your internal disk, named `4tbhdd`, is now mounted at `/mnt/4tbhdd` and is automatically decrypted during the boot process.

## Step 17: Double Verify Status

To double-check the status of the encrypted device, you can use the `cryptsetup status` command. This will provide detailed information about the LUKS device:

```shell
cryptsetup status /dev/mapper/4tbhdd
```

The output should confirm that `/dev/mapper/4tbhdd` is active, and it is in read/write mode.

## Conclusion

Congratulations! You've successfully encrypted and set up an internal disk for decryption using a keyfile alongside a passphrase. This adds an extra layer of security to your storage solution. Remember to store your keyfile securely and keep it accessible to ensure smooth disk decryption.

## References

- [How to encrypt a drive and mount it automatically at boot / no prompt](https://onion.tube/watch?v=UXJrSji-nNo)
- [How to automatically mount an encrypted volume at boot?](https://discussion.fedoraproject.org/t/how-to-automatically-mount-an-encrypted-volume-at-boot/71271/1)
- [How to add a passphrase, key, or keyfile to an existing LUKS device](https://access.redhat.com/solutions/230993)
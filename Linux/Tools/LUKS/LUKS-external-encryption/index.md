---
author: "UmmIt"
title: "Encrypting External Drives with LUKS: A Beginner's Guide (LVM Not Included)"
description: "This article will guide you through the process of encrypting an external drive with LUKS and mounting it on your GNU/Linux system."
tags: ["LUKS", "Encryption", "Security", "GNU/Linux", "External Drive"] 
date: 2024-12-29T21:11:06+0800
---

## Introduction

Encrypting your external drive is a great way to protect your data from unauthorized access in case the drive is lost or stolen. LUKS (Linux Unified Key Setup) is a popular disk encryption method that allows you to encrypt your drives with ease. This article will guide you through the process of encrypting an external drive with LUKS and mounting it on your GNU/Linux system.

### Prerequisites

Before you begin, make sure you have the following:

- An external drive that you want to encrypt.
- A GNU/Linux system with the `cryptsetup` package installed.

## Step 1: Identify the External Drive

First, you need to identify the device identifier of your external drive. You can do this by plugging in the drive and running the following command:

```bash
lsblk
```

## Step 2: Create a filesystem on the External drive

Before encrypting the drive, you need to using tool like gdisk or fdisk to initialize the disk and create a partition. For example, to create a partition on `/dev/sdb`, you can use the following command:

```bash
sudo gdisk /dev/sdb
o # Create a new empty GUID partition table (GPT)
n # Create a new partition
<Enter> # Use the default partition number
<Enter> # Use the default first sector
<Enter> # Use the default last sector
t # Change the partition type
L # List known partition types
<Enter> # Choose the Linux filesystem type (e.g., 8309 for LUKS)
w # Write changes to disk
```

In case you lsblk again don't show the new partition, you can run the following commands for the system to recognize the new partition:

```bash
sudo sync
sudo partprobe /dev/sdb
```

And now `lsblk` should show the new partition, for example `/dev/sdb1`.

## Step 3: Encrypt the External Drive

To encrypt the external drive, use the `cryptsetup` command with the `luksFormat` option. Replace `/dev/sdb1` with the actual device identifier of your external drive:

```bash
cryptsetup luksFormat --type luks2 --cipher aes-xts-plain64 --hash sha256 --iter-time 10000 --key-size 256 --pbkdf argon2id --use-urandom --verify-passphrase /dev/sdb1

YES
```

You will be prompted to enter a passphrase for the encryption.

## Step 4: Open the Encrypted drive

After encrypting the drive, you need to open it using the `cryptsetup open` command. Replace `/dev/sdb1` with the actual device identifier of your encrypted drive and `yourdrive` with a name you want to assign to the mapped device:

```bash
sudo cryptsetup open /dev/sdb1 yourdrive
```

You will be prompted to enter the passphrase you set during encryption.

## Step 5: Create a Filesystem on the Mapped device

Now that the encrypted drive is open, you can create a filesystem on the mapped device. For example, to create an btrfs filesystem on the mapped device `/dev/mapper/yourdrive`, you can use the following command:

```bash
sudo mkfs.btrfs /dev/mapper/yourdrive
```

## Step 6: Mount the Encrypted Drive

Finally, you can mount the encrypted drive to a directory of your choice. For example, to mount the mapped device `/dev/mapper/yourdrive` to the `/mnt/encrypted` directory, you can use the following command:

```bash
mount --mkdir /dev/sdb1 /mnt/yourdrive
```

You can now access the encrypted drive at the specified mount point. To ensure that the drive is mounted you can type 'lsblk' and see if the drive is mounted.


## Close the Encrypted drive

To close the encrypted drive, you can use the `cryptsetup close` command. Replace `yourdrive` with the name you assigned to the mapped device:

```bash
sudo cryptsetup close yourdrive
```

## Cannot Unmount the Drive

If you are unable to unmount a drive, it is likely that the drive is still in use. You can follow these steps to identify and resolve the issue:

### Step 1: Check for Open Files

Use the `lsof` command to check for any open files on the mount point:

```bash
sudo lsof +D /mnt/yourdrive
```

### Step 2: Identify Processes Using the Mount Point

You can also use the `fuser` command to identify which processes are using the mount point:

```bash
sudo fuser -m /mnt/yourdrive
```

### Step 3: Terminate Specific Processes

If you need to terminate a specific process using the mount point, you can use the following command:

```bash
sudo kill -9 PID
```

### Step 4: Kill Processes Using the Mount Point

If you don't sure which process is using the mount point and don't care about those processes, you can kill all processes using the mount point, which is all processes using the drive, all the using files will be closed.

```bash
sudo fuser -k /mnt/yourdrive
```

## Step 5: Unmount the Encrypted Drive

Once you have closed all processes using the mount point, you can unmount the encrypted drive using the following command:

```bash
sudo umount /mnt/yourdrive
```

## Step 6: Close the Encrypted Drive

Finally, you can close the encrypted drive using the `cryptsetup close` command:

```bash
sudo cryptsetup close yourdrive
```

## Don't Force Unplugging the Drive

Gernerally, many people forget to unmount the drive before closing the encrypted drive, which will cause the drive to be still in use. And just foce to unplugging the drive will cause the drive to be corrupted. So, it is important to unmount the drive before closing the encrypted drive.

## Conclusion

That's all! You have successfully encrypted an external drive with LUKS and mounted it on your GNU/Linux system. Remember to safely unmount and close the encrypted drive before disconnecting it to avoid data corruption.

---
title: "Resizing LVM and LUKS Encrypted Btrfs Filesystem"
author: "UmmIt"
tags: ["BTRFS", "LUKS", "Linux"]
date: 2023-12-22T04:23:37+0800
---

## Introduction

Managing disk space efficiently is crucial for maintaining a well-organized system. In this guide, we'll explore the steps to resize a Btrfs filesystem within an LVM and LUKS encrypted setup. This process involves reducing, extending, and fixing the size of the filesystem to meet your storage needs.

Also, Btrfs enables you to resize the filesystem online without disruption. Alternatively, you might need to create a bootable ISO using a live-ISO and then initiate the resizing of the filesystem.

### Prerequisites

Before proceeding, ensure you have a basic understanding of LVM (Logical Volume Manager) and LUKS (Linux Unified Key Setup) encryption.

>**And backup your DATA First!!!!!**

## Step 1: Reduce Btrfs Filesystem

To reduce the size of the Btrfs filesystem, use the `btrfs filesystem resize` command. Make sure to specify the target size and the mount point.

```bash
sudo btrfs filesystem resize -500G /mnt/home
```

Here, we reduce the filesystem mounted at `/mnt/home` by 500GB. Adjust the size as needed for your scenario.

## Step 2: Reduce LVM Logical Volume

To match the reduced Btrfs filesystem size, you must shrink the corresponding LVM logical volume. Use the `lvreduce` command, specifying the new size and the path to the logical volume.

```bash
sudo lvreduce -L 500G /dev/vol/home
```

Replace `/dev/vol/home` with the path to your logical volume. This step ensures that the LVM logical volume aligns with the resized Btrfs filesystem.

## Step 3: Extend LVM Logical Volume

If you need to revert the changes or allocate additional space to the filesystem, use the `lvextend` command. This command allows you to increase the size of the LVM logical volume.

```bash
sudo lvextend -l +100%FREE /dev/vol/home
```

Here, we extend the logical volume to utilize all available free space. Adjust the logical volume path as needed.

## Step 4: Verification

To verify the changes, use the following commands:

- Verify Btrfs filesystem size:

```bash
sudo btrfs filesystem df /mnt/home
```

- Verify LVM logical volume size:

```bash
sudo lvdisplay /dev/vol/home
```

Ensure that the sizes align with your expectations.

## Conclusion

Resizing a Btrfs filesystem within an LVM and LUKS encrypted setup requires a careful and systematic approach. By following these steps, you can efficiently manage your storage space, whether you need to reduce, extend, or fix the size of the filesystem. Always double-check the sizes and perform these operations with caution to avoid data loss.

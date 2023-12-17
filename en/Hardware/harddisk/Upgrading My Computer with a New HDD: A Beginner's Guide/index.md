---
author: "UmmIt"
title: "Upgrading My Computer with a New HDD: A Beginner's Guide"
description: ""
tags: ["Harddisk" ]
date: 2023-12-07T19:19:20+0800
thumbnail: https://toshiba.semicon-storage.com/content/dam/toshiba-ss-v3/master/en/storage/product/data-center-enterprise/mg08ada.jpg
---

## Introduction

I recently decided to upgrade my computer by adding a new HDD. This was a significant step for me as I hadn't worked much with hardware before and hadn't installed any HDD in my computer.

>`("Toshiba Enterprise Series MG08ADA400N 4TB 7200rpm 256MB 3.5")`

![New HDD](./featured.webp)

## Why Upgrade?

Initially, I hesitated due to concerns about storage space, but with the increasing number of virtual machines on my computer, especially after the GPU passthrough, I finally decided to invest in a new HDD. Today, I purchased a 4TB HDD, and I'm excited to share my experience of installing hardware for the first time.

## How to Install the HDD: A Beginner's Guide

Having watched several YouTube videos that weren't beginner-friendly, I sought advice from the online community. The entire process took around 5 hours, and I learned valuable lessons on how to install an HDD.

### Step 1: Get Ready

Remove the left and right side plates from the computer case. Identify the HDD socket on the motherboard, the motherboard SATA socket, and the power supply unit socket.

![Port](https://pcguide101.com/wp-content/uploads/2021/07/hard-drive-ports-connections.jpeg)

![HDD Port](https://images.easytechjunkie.com/sata-cable-connected-to-a-drive.jpg)

### Step 2: Find the Cables

You'll need two cables for the installation—usually found in the motherboard and power supply boxes.

#### Power Supply Unit Cable

This cable supplies power to the HDD.

![PSU](https://ae01.alicdn.com/kf/HLB1JJ26XvLsK1Rjy0Fbq6xSEXXaX/PCIe-6Pin-Male-to-2-3-4-SATA-Power-Supply-Cable-for-Seasonic-Focus-Plus-Platinum.jpg)

#### SATA Cable

This cable is for data transfer.

![SATA Cable](https://pactech-inc.com/wp-content/uploads/2014/06/SATA2K-XXL-Amphenol-SATA-Cable-Straight-with-Latch-to-Left-Angle-003.jpg)

### Step 3: Plug in the Cables

Ensure the power supply unit and SATA cables are plugged in correctly. Connect the power supply unit cable to the corresponding port on your PSU, and connect the SATA cable to one of the SATA 3-5 ports on the motherboard.

![SATA-Destination](https://pcguide101.com/wp-content/uploads/2021/06/SATA-2-700x525.jpeg)

![PSU-Destination](https://pbs-prod.linustechtips.com/monthly_2021_08/1117139249_Corsairpsu.png.45d19ada31b0572c874aebba2a36f041.png)

### Step 4: Test the HDD

Reconnect all cables (PSU, HDMI, USB, keyboard, etc.), turn on your computer, and check if the HDD is correctly displayed using the `lsblk` command in the terminal.

```shell
❯ lsblk
NAME           MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda              8:0    0   3.6T  0 disk <- This is my new HDD
```

### Step 5: Secure the HDD

Ensure the HDD is correctly placed in the rack inside the computer case. Lock the HDD rack securely.

## Conclusion

This beginner's guide aims to simplify the process of installing an HDD for those, like me, who are new to hardware upgrades. With your new HDD successfully installed, you'll have expanded storage and improved capabilities for your computer.

## Reference

- [What Does a SATA Port Look Like?](https://pcguide101.com/motherboard/what-does-a-sata-port-look-like/)
---
title: "Brute-Force Attack on MD5 Hash Using Hashcat"
date: 2024-10-28T09:10:10+0800
tags: ["Brute-force", "Cyber-Security", "Hashcat", "MD5"]
lastmod: 2024-12-31T06:45:40+0800
---

## Introduction

MD5 is a cryptographic hash function that produces a 128-bit hash value, and its one-way function. That means you can't reverse the hash value to the original value. But you can break it using brute-force attack. In this article, I’ll guide you through the process of breaking an MD5 hash using Hashcat on Arch Linux or any other GNU/Linux distribution.

>NOTICE: I'll not cover any guide on OSX or windows. Use GNU/Linux instead.

## Prerequisites

Before we start, you’ll need a wordlist for the brute-force attack. You can easily download a wordlist from the Arch User Repository (AUR) with the following command:

```bash
paru -S wordlists
```

The downloaded wordlists will be located at `/usr/share/wordlists` and will be about 2.1 GB in size.

## Installing Hashcat

Install Hashcat from the official Arch Linux repository using the `pacman` command:

```bash
sudo pacman -S hashcat rocm-hip-sdk rocm-opencl-sdk
```

## Using Hashcat

To begin the brute-force attack on the MD5 hash, use the following command:

```bash
hashcat -m 0 -a 0 -d 2 <value_of_md5> <wordlist_file>
```

This process may take some time, depending on the complexity of the hash and the size of your wordlist.

Once the process is complete, you can view the cracked value of the MD5 hash using:

```bash
hashcat -m 0 -a 0 <value_of_md5> --show
```

## Advantages of Hashcat

Hashcat is particularly useful for batch processing multiple hash values. You can leverage the `find` command to automate the process for all hashes in a specified directory. For example:

```bash
find /usr/share/wordlists/ -type f -exec hashcat -m 0 -a 0 -d 2 hash {} \;
```

This command will execute Hashcat for each file in the specified directory, making it an efficient way to handle multiple hashes.

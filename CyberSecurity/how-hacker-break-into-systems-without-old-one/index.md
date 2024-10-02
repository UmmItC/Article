---
title: "How Hackers Break into Systems: Resetting Passwords Without the Old One"
date: 2024-08-26T20:03:30+0800
tags: ["Ethical-hacking", "Cyber-Security", "Windows", "Password-reset"]
---

## Introduction

This article is for educational purposes only. Please use this knowledge responsibly. Unauthorized access to systems is illegal and unethical. The methods here are used for legitimate recovery and testing, not for illegal activities.

This article is a method on using the same operating system to break into the system file. Not using other system to mounting the file you want.

## Why?

Sometimes people forget their system password. While there are proper ways to recover or reset a password, this guide will show you a basic method that was common in the past.

## Is it Easy?

Yes, it can be surprisingly easy with the right tools. Here's a simple overview:

1. **Use a bootable USB drive with Windows on it.**
2. **Replace a critical system file (`utilman.exe`) with `cmd.exe`.**
3. **Restart the system and use the modified file to open a command prompt.**
4. **Use the command prompt to create a new admin user or reset the password.**

## Step 1: Create a Bootable Windows USB

You need a USB drive with a bootable Windows ISO. Search online for guides if you’re not sure how to do this.

## Step 2: Boot from the USB

Insert the USB drive and restart your computer. Use F12 or F11 to choose the USB drive as the boot device.

## Step 3: Open Command Prompt

When you see the Windows Setup screen, press `Shift + F10` to open a command prompt.

## Step 4: Find the Right Drive

The command prompt starts in `X:\Sources` or a similar directory. You need to find your main system drive, usually `C:\`. Use the command:

```cmd
wmic logicaldisk get name
```

Switch to each drive and use `dir` to find the `Windows` folder and `cd` again into the `System32` folders. 

```cmd
C:\
dir

...
# more precise
cd Windows
cd System32

# One line
cd Windows\System32
```

Repeat with `D:\` and others until you find the correct drive.

## Step 5: Rename `utilman.exe`

In the `System32` directory, rename `utilman.exe` to something else:

```cmd
ren utilman.exe utilman2.exe
```

## Step 6: Replace `utilman.exe` with `cmd.exe`

Copy `cmd.exe` and rename it to `utilman.exe`:

```cmd
copy cmd.exe utilman.exe
```

## How Does This Work?

You’ve replaced the utility manager (which appears on the login screen) with a command prompt. 

## Step 7: Restart the System

Exit the Windows Setup and restart your computer. At the login screen, click the Utility Manager icon (near the power button). You’ll see a command prompt instead of the usual utility manager.

## Final Step: Reset the Password

In the command prompt, reset the password with:

```cmd
net user <username> <new_password>
```

Replace `<username>` with the actual username and `<new_password>` with your new password.

## Conclusion

After resetting the password, don’t forget to restore the original `utilman.exe` file. Use these commands to put things back:

```cmd
del utilman.exe
ren utilman2.exe utilman.exe
```

## Another ways

There are other, more complex methods using like `Mimikatz` or `ophcrack`, but they require more advanced knowledge and skills.

This method should be straightforward if you have experience with ethical hacking or CTF challenges.

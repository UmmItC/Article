---
author: "UmmIt"
title: "Fixing Security Vulnerabilities in systemd-boot (/boot) - No longer accessible to normal users: A Step-by-Step Guide"
description: "Learn how to enhance your Linux system's security by fixing vulnerabilities in systemd-boot (/boot). Follow this step-by-step guide to restrict access, ensuring that critical boot files are no longer accessible to normal users, bolstering your system's defenses."
tags: ["Linux", "Bootctl", "Boot-loader"]
date: 2023-11-07T13:18:30+0800
---

## Introduction

Your bootloader should not have access granted to ordinary users. If your regular user is also capable of reading `/boot` or `/efi`, then it is recommended to prevent access. So, do it with few commands!

```shell
Copied "/usr/lib/systemd/boot/efi/systemd-bootx64.efi" to "/efi/EFI/systemd/systemd-bootx64.efi".
Copied "/usr/lib/systemd/boot/efi/systemd-bootx64.efi" to "/efi/EFI/BOOT/BOOTX64.EFI".
⚠️ Mount point '/efi' which backs the random seed file is world accessible, which is a security hole! ⚠️
⚠️ Random seed file '/efi/loader/random-seed' is world accessible, which is a security hole! ⚠️
Random seed file /efi/loader/random-seed successfully refreshed (32 bytes).
Created EFI boot entry "Linux Boot Manager".
```

## What's problem?

The problem with your `/boot` directory is that normal user can access it. One concern is that even though normal users cannot edit the directory, but still can see the `/boot` path without requiring root access.

>I THINK, `/BOOT` PATH SHOULD ONLY BE ABLE FOR ROOT USER ACCESS. EVEN ONLY READ NOT WRITE.

### So what should you do now?

just changing the permissions for your `/boot` directory. Here's how to do!

## FAT32

If the bootloader path is formatted to fat32, changing the permission may not be a simple task. To address this, fstab editing with additional options is necessary. Here's how:

### Step 1: Edit fstab Configuration

1. Open the fstab configuration file using your preferred text editor (for instance, vim).

   ```shell
   sudo vim /etc/fstab
   ```

2. Add `fmask=0137,dmask=0027` to the `/boot` partition options as shown:

```shell
# /dev/sda1

UUID=xxxx-xxxx      /boot       vfat        rw,relatime,fmask=0137,dmask=0027,errors=remount-ro     0 2
```

3. Save the file and exit the editor.

### Step 2: Adjust Permissions

FStab is a table that identifies which partition should initiate automatically when the system boots up.

If you do not restart your system after applying changes, your system will remain in its previous state. This table is loaded during system boot-up.

1. Reboot your system to apply the changes made in the fstab configuration.

2. After rebooting, change the permissions for the random seed file to restrict world access.

   ```shell
   sudo chmod o-rwx /boot/loader/random-seed
   ```

3. Similarly, ensure the `/boot` directory is secure by adjusting its permissions.

   ```shell
   sudo chmod o-rwx /boot
   ```

4. Now, after updating bootctl as shown below, attempt to access the `/boot` directory as a normal user. You will no longer be able to access this directory, making it safe now!

    ```shell
    $ sudo bootctl --path=/boot/ install

    [sudo] password for user: 
    Copied "/usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed" to "/boot/EFI/systemd/systemd-bootx64.efi".
    Copied "/usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed" to "/boot/EFI/BOOT/BOOTX64.EFI".
    Random seed file /boot/loader/random-seed successfully refreshed (32 bytes).
    Created EFI boot entry "Linux Boot Manager".
    ```

## Conclusion

By following these steps, you have successfully eliminated the security vulnerabilities in systemd-boot. Your Linux system is now shielded from unauthorized access, enhancing the overall security of your computing environment. Stay vigilant and proactive in maintaining your system's security to enjoy a seamless and protected Linux experience.

## Reference

- [bootctl install” outputs some warnings about /efi mount point and random seed file in the terminal](https://forum.endeavouros.com/t/bootctl-install-outputs-some-warnings-about-efi-mount-point-and-random-seed-file-in-the-terminal/43991)

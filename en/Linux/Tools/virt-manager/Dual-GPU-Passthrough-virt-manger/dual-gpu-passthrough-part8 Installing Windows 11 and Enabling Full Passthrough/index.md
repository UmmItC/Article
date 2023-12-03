---
author: "Arcsly"
title: "Dual GPU Passthrough Guided Part 8: Installing Windows 11"
description: "Set up a Windows 11 virtual machine using QEMU/KVM and enable a virtual Trusted Platform Module (TPM) for successful installation, even on systems without native TPM support."
tags: ["QEMU/KVM", "GPU-Passthrough", "virt-manager", "win11"]
date: 2023-08-14T10:34:50+0800
thumbnail: https://www.howtogeek.com/wp-content/uploads/2021/06/windows_11_generic_hero_1.jpg?height=200p&trim=2,2,2,2
lastmod: 2023-12-03T14:01:05+0800
---

## Introduction

Windows 11 introduces a modern interface and various features to the Windows operating system. However, installing Windows 11 on a virtual machine, particularly dealing with the TPM 2.0 requirement, can be challenging. In this guide, we'll take you through the process of installing Windows 11 on QEMU/KVM virtualization, complete with a virtual TPM solution. This enables you to experience Windows 11 on systems lacking native TPM support.

## Before You Begin

Ensure you've completed the crucial steps outlined in our comprehensive guide:

### 1. Dual GPU Passthrough Guide - Part 1 to Part 6

This guide provides essential details on configuring your Linux system for virtualization, significantly enhancing your Windows 11 installation experience.

Read Part 1 to Part 6: [Tags gpu-passthrough](/tags/gpu-passthrough/)

### 2. Secure Boot Must Be Enabled

Refer to this [guide](/posts/en/linux/system/uefi/secure-boot/how-to-enable-secure-boot-with-self-cert/) to enable Secure Boot on your Linux machine, as Windows 11 requires Secure Boot.

## Step 1: Installing Required Packages

After successfully configuring, install the necessary packages for setting up the virtual TPM. Open your terminal and execute the following command:

```bash
sudo pacman -S swtpm
```

These packages provide the components required to create a virtual TPM for Windows 11 installation.

## Step 2: Create New VM

1. In the VM manager, create a new VM. Select `Local Install media`.

![Local install media](./1.xofx/1of5.png)

2. Choose your Windows ISO.

![Windows ISO](./1.xofx/2of5.png)

3. Configure memory and CPU settings.

![Memory and CPU](./1.xofx/3of5.png)

4. Enable storage for this VM.

![Enable Storage](./1.xofx/4of5.png)

5. Name your VM and click `Customize configuration before install`.

![Customize Configuration](./1.xofx/5of5.png)

## Step 3: Virt-manager Configuration

Now, configure the Firmware, CPU, and other settings:

1. In the Overview section, select `Firmware` to `UEFI x86_64: /usr/share/edk2/x64/OVMF_CODE.secboot.fd`.

![Firmware](./2.virt-manager%20device%20setup/Firmware.png)

2. CPUs: Set the CPU configuration to `host-passthrough`. Enable `Copy host CPU configuration (host-passthrough)`, `Manually set CPU topology`, and select your preferred number.

![CPU](./2.virt-manager%20device%20setup/CPU%20Select.png)

3. VirtIO Disk: Create a storage for `VirtIO`, set it to `40GB`, and configure the bus type as `VirtIO` with the device type as `Disk device`.

![Add VirtIO Disk](./2.virt-manager%20device%20setup/VirtIO%20Disk.png)

4. VirtIO ISO: Create a storage for `VirtIO ISO` and set it to CDROM device.

![Add VirtIO ISO](./2.virt-manager%20device%20setup/VirtIO%20iso.png)

5. Boot Options: Set `SATA CDROM 1` to the top. This storage contains the Windows 11 ISO, making it the primary boot option.

6. TPM: Add a TLS and version 2.0 of the TPM.

![TPM](./2.virt-manager%20device%20setup/tls-tpm.png)

7. Remove NIC: Remove the NIC to install Windows offline, bypassing Microsoft Account requirements during installation. You can later use the command-line to proceed without connecting to the network.

## Step 4: Booting into System

Click the `Begin Installation` button to start the machine. If you miss the prompt to press any key for Windows 11 ISO boot-up, follow these steps:

1. **UEFI Shell:** The Shell automatically loads. Type `exit` and press `Enter` to leave the BIOS menu.

![Exit BIOS](./3.%20boot-manager/exit%20bios.png)

2. After exiting, use the arrow keys to navigate. Select `Boot Manager`.

![Boot Manager](./3.%20boot-manager/Boot-manger-exit.png)

3. Enter the first option. The UEFI DVD-ROM should be the first one if you followed my steps.

![Boot Manager](./3.%20boot-manager/boot-manager-exit-2.png)

4. Press any key to launch the Windows 11 ISO.

![Press any key](./3.%20boot-manager/Pressanykey-one.png)
![Win11 Launch](./3.%20boot-manager/Loading-one.png)

## Step 5: Windows Setup

Now, you should see the Windows setup page. Follow these steps:

1. **Language and Settings:** Select your preferred language, time, and keyboard layout. Click next.

![Win11 Setup](./4.%20windows%20setup/w11.png)

2. Click `Install now`.

![Win11 Install Now](./4.%20windows%20setup/w11-install-now.png)

3. Click `I Don't have the product key`.

![Win11 Product Key](./4.%20windows%20setup/w11-i-dont-have-product-key.png)

4. **Select Windows 11 Pro:** Choose the Pro version for additional features. Click `Next`.

![Win11 Pro Version](./4.%20windows%20setup/w11-select-pro.png)

5. Accept the LICENSE.

![Win11 Accept License](./4.%20windows%20setup/w11-i-accpect-license.png)

6. **Custom Installation:** Apply the VirtIO Driver. Select `Custom: Install Windows only (advanced)`.

![Win11 Custom Installation](./4.%20windows%20setup/w11-custom-disk.png)

7. **Load Driver:** Click `Load driver`.

![Win11 Load Driver](./4.%20windows%20setup/w11-load-driver.png)

8. **Browse:** Click `Browse`.

![Win11 Select Browse](./4.%20windows%20setup/w11-select-driver.png)

9. Find the Win11 driver at `E:\amd64\w11`. Click OK.

![Win11 Select Drivers](./4.%20windows%20setup/w11-amd64-win11.png)

10. After the driver loads, select the REDHAT result. Click `Next`.

![Win11 REDHAT Driver Next](./4.%20windows%20setup/w11-driver-RedHAT.png)

Wait for the driver installation to complete.

![Win 11 Waiting the installation compelete](./4.%20windows%20setup/w11-REDHAT-driver-load.png)

11. After setup, install drivers on your disk (e.g., `400GB`).

![Win11 Install Disk](./4.%20windows%20setup/w11-install.png)
![Win11 Installing](./4.%20windows%20setup/w11-installing.png)
![Win11 Installing Restart](./4.%20windows%20setup/w11-restarting.png)

12. The Windows installation will reboot several times. Ignore the `Press any key` prompt; it's part of the installation process.

![Win11 Ignore Press any key](./4.%20windows%20setup/w11-ignore-installing.png)
![Win11 Installing](./4.%20windows%20setup/w11-installing-2.png)
![Win11 Loading Installing](./4.%20windows%20setup/w11-loading.png)

When you see the `Hi` screen, the Windows installation is almost finished.

## Step 5: Personal Configuration Setup with Offline

Now, you should see the section showing the Country select page. Press `Shift + F10` to launch `cmd` and type:

```cmd
OOBE\BYPASSNRO
```

After entering the command, the system will automatically reboot. Once your system boots up again, you can proceed with the final installation steps.

![OOBE\BYPASSNRO](./5.%20installing-config/w11-bypassnro.png)

## Step 6: Power Off the System

Congratulations! Windows 11 installation is complete.

![Win11 Installed](./6.%20installed/w11-installed.png)

Power off your system. This step is crucial before proceeding to insert fake motherboard information.

![Win11 Power Off](./6.%20installed/poweroff.png)

## Step 7: Insert Fake Motherboard Information

To insert fake motherboard information, follow the steps outlined in this [article](/posts/en/linux/tools/virt-manager/virt-manager-fake-motherboard-detail/).

## Step 8: Insert GPU

Now, it's time to insert your GPU and GPU-Audio into your machine. Remove the Windows 11 ISO and add the NIC (network).

Add GPU:

![Win11 Add GPU](./8.%20Add%20GPU/w11-add-gpu.png)

Add GPU-Audio:

![Win11 Add GPU-Audio](./8.%20Add%20GPU/w11-add-gpu-audio.png)

Add NIC:

![Win11 Add NIC](./8.%20Add%20GPU/w11-add-nic.png)

Remove ISO:

![Win11 Remove Win11 ISO](./8.%20Add%20GPU/w11-remove-iso.png)

## Step 9: Update Windows 11

Power on your Windows 11 machine and update your Windows to ensure you have the latest updates.

![Win11 Check for Update](./9.%20update-win11/w11-makesure-uptodated.png)

### Update Error

If you encounter any update errors, let the system automatically reboot; this is a common Windows issue.

![Win11 Update Error](./9.%20update-win11/w11-error.png)

Make sure your system is up-to-date.

![Win11 Up to Date](./9.%20update-win11/w11-makesure-uptodated.png)

## Step 10: Install VirtIO Drivers

Proceed to install the VirtIO drivers. Search for `Device Manager` and open it.

![Device Manager](./10.%20VirtIO-Load-drivers/w11-Device-manger.png)

In the Device Manager, you will see a warning icon with `PCI Devices`.

![Device Manager Overview](./10.%20VirtIO-Load-drivers/w11-device-manger-overview.png)

Right-click on it and select `Update driver`.

![Device Manager Update PCI](./10.%20VirtIO-Load-drivers/w11-device-manger-update-pci.png)

Choose `Browse my computer for drivers`.

![Device Manager Search Driver](./10.%20VirtIO-Load-drivers/w11-device-manager-search-driver.png)

Select `E:\` (your VirtIO ISO), click `OK` and `Next`.

![Device Manager Search E Driver](./10.%20VirtIO-Load-drivers/w11-device-manger-search-e-driver.png)

Windows will automatically search for drivers in your `E:\` drive.

![Device Manager Searching E](./10.%20VirtIO-Load-drivers/w11-device-manager-searching-e.png)
![Device Manager Loaded](./10.%20VirtIO-Load-drivers/w11-device-manger-loaded.png)

All the VirtIO drivers are now loaded into your system.

![Device Manager Done](./10.%20VirtIO-Load-drivers/w11-device-manger-done.png)

## Step 11: Install VirtIO Driver gt

To enhance your computer's virtualization performance, it is recommended to install the VirtIO Driver gt. Follow these simple steps:

1. Launch Explorer.

2. Locate `E:\virtio-win-gt-x64.msi` and start the installation.

   ![Explorer E VirtIO Win X64](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64.png)

3. The installation steps are straightforward; click `Next` through the wizard.

   ![Explorer E VirtIO Win X64 Next](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-next.png)
   ![License VirtIO](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-license.png)
   ![Explorer E VirtIO Win X64 Select](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-select.png)
   ![Explorer E VirtIO Win X64 Install](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-install.png)

4. The installation may require superuser permission; click `Yes.`

   ![Explorer E VirtIO Win X64 Permission](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-premission.png)

   ![Explorer E VirtIO Win X64 Installing](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-installing.png)
   ![Explorer E VirtIO Win X64 Finish](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-finish.png)

5. Next, install the guest tools.

   ![Explorer E VirtIO Win X64 Guest Tool](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-guest-tool.png)
   ![Explorer E VirtIO Win X64 Guest Tool Install](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-guest-tool-install.png)

   ![Explorer E VirtIO Win X64 Guest Tool Install Yes](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-guest-tool-install-yes.png)

   Just click `Yes.`

   ![Explorer E VirtIO Win X64 Guest Tool Installing](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-guest-tool-installing.png)
   ![Explorer E VirtIO Win X64 Guest Tool Success](./11.%20VirtIO-install-drivers/explorer-e-virt-win-x64-guest-tool-sucess.png)

## Step 12: Enable AMD GPU

Windows 11 might require you to manually enable the GPU in Device Manager. After enabling, you can use your second screen to control your VM.

![Enable GPU](./11.%20amd-driver/enable-amd-driver.png)

## Step 13: Install GPU Drivers

Visit your GPU manufacturer's website, such as AMD, Nvidia, or Intel, and install the GPU drivers. The installation should not encounter any errors, given that you have successfully set up the fake motherboard.

## Optional: Enhance Windows Speed

Consider trying something like `ReviOS` to remove unnecessary elements from your Windows 11 installation. Check out this [article](/posts/en/windows/custom/revios/) for more details!

## Conclusion

Congratulations! You've now completed the installation of Windows 11 on a QEMU/KVM virtual machine, complete with a virtual TPM, bypassing the native TPM requirement. This comprehensive guide equips you with the necessary steps to enjoy Windows 11's features within a virtualized environment, even on systems without TPM support. Explore Windows 11 and leverage its new capabilities seamlessly integrated into your Linux host system.

## Reference

- https://www.smoothnet.org/qemu-tpm/
- https://ivonblog.com/posts/qemu-kvm-secure-boot-tpm-20/
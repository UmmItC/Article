---
author: "UmmIt"
title: "How to Add More Keys and Verify Keys on LUKS"
description: "This article will guide you through the process of adding a new key and verifying it on LUKS-encrypted."
tags: ["LUKS", "Encryption", "Security", "GNU/Linux"] 
date: 2024-11-24T21:42:06+0800
---

## Introduction

If you're using LUKS (Linux Unified Key Setup) for disk encryption, you might already have one or more passphrases set up for accessing your encrypted volumes. LUKS allows you to manage up to **eight key slots** (numbered 0 to 7), which means you can add additional passphrases for convenience or security purposes. This article will guide you through the process of adding a new key and verifying it.

### Prerequisites

Before proceeding, ensure that you already have at least one LUKS key set up on your device. This is essential because you'll need an existing passphrase to authenticate when adding a new one.

### Adding a New Key

To add a new passphrase to your LUKS-encrypted volume, use the `cryptsetup luksAddKey` command. Here’s how to do it:

1. **Open a terminal.**
2. **Execute the following command:**

   ```bash
   sudo cryptsetup luksAddKey /dev/nvme0n1p2
   ```

   Replace `/dev/nvme0n1p2` with the actual device identifier of your encrypted volume.

3. **Enter an existing passphrase** when prompted. This is necessary for authentication.
4. **Type the new passphrase** you want to add and confirm it.

This command adds the new passphrase to the next available key slot on the specified LUKS volume.

### Verifying the Key Added

To ensure that the new key has been successfully added, you can use the `cryptsetup luksDump` command:

```bash
sudo cryptsetup luksDump /dev/nvme0n1p2
```

This command will display detailed information about your LUKS volume, including the status of each key slot. You should see that one of the key slots is now filled with your newly added passphrase.

### Testing the Passphrase Without Rebooting

After adding a new passphrase, it's a good idea to test it immediately without rebooting your system. You can do this using the following command:

```bash
cryptsetup -v open --test-passphrase --type luks /dev/nvme0n1p2
```

Make sure to replace `/dev/nvme0n1p2` with the correct device identifier for your encrypted volume. Enter the new passphrase when prompted. If successful, this indicates that your new key is functioning correctly.

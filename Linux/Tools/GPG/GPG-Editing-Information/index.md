---
author: "UmmIt"
title: "Editing GPG Key Information: A Quick Guide"
tags: ["GPG", "Linux"]
date: 2023-12-31T09:16:21+0800
thumbnail: https://cdn2.iconfinder.com/data/icons/system-flat-buttons/512/gnu-1024.png 
---

## Introduction

First of all, Happy New Year!! :3 This is the first post for the last day of 2023 :D **2023-12-31**

**Updating Email, User Name, or Expiry Date**: Here's a simple guide on how to edit details like email, user name, and expiry date.

>**Note:** When updating email information, ensure you add a new email before attempting to delete the old one. This maintains the key's integrity and allows for a smooth transition.

### **Step 1: Find the Key ID**
   Start by identifying the key ID using the following command:
   ```shell
   gpg --list-secret-keys --keyid-format=long
   ```

### **Step 2: Edit the Key**
   Enter the GPG key editing mode with the key ID:
   ```shell
   gpg --edit-key <ID>
   ```
   On the GnuPG prompt, proceed to the next steps.

### **Step 3: Adding a New Email and Deleting the Old One**
   Inside the GnuPG prompt:
   ```shell
   gpg> adduid
   ```
   Follow the interactive prompts to provide the new details. Confirm and enter the passphrase when prompted. Save the changes:
   ```shell
   gpg> save
   ```
   To delete the old email, ensure you have added a new email first:
   ```shell
   gpg> deluid
   ```

### **Step 4: Updating Expiry Date**
   To change the expiry date:
   ```shell
   gpg> expire
   ```
   Follow the interactive prompts for details. To modify the ssb expiry date, select the uid (for example, uid 1):
   ```shell
   gpg> uid 1
   gpg> expire
   ```
   Adjust the date and save:
   ```shell
   gpg> save
   ```

### **Step 5: Verify Changes**
   Confirm that the changes are correct by checking the signed commit messages:
   ```shell
   git log --show-signature
   ```

## Conclusion

This step-by-step guide you to confidently navigate the process of editing GPG key information, whether you need to update your email, user name, or expiry date.

## Reference

- [Can I add an email address to an existing GPG key?](https://security.stackexchange.com/questions/261467/can-i-add-an-email-address-to-an-existing-gpg-key)

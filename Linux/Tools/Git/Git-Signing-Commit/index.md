---
author: "UmmIt"
title: "Git: Secure Committing with GPG"
tags: ["Git"]
date: 2023-12-27T08:20:15+08:00
thumbnail: https://www.pngrepo.com/png/349319/512/codeberg.png
---

## Introduction

Ensure the security of your commits by following these simple steps to set up GPG key signing. Add an extra layer of protection to your Git repositories with this quick and easy guide.

### Step 1: Install Necessary Packages

Start by installing the required packages, GnuPG, and pinentry:

```shell
sudo pacman -S gnupg pinentry
```

### Step 2: Generate a GPG Keypair

Generate a GPG keypair with the following command. Follow the prompts and enter information consistent with your GitHub/GitLab/Codeberg/Gitea account:

```shell
gpg --full-generate-key
```

- Choose RSA for enhanced security.
- Opt for a key size of 4096 for better security.
- Set the key expiration (0 for no expiration).
- Enter information matching your account details.
- Set a passphrase for the GPG keys.

### Step 3: Retrieve the Public Key

Get your GPG key's information using the following command:

```shell
gpg --list-secret-keys --keyid-format LONG
```

Copy the GPG key ID (the `sec` value, not `ssb`). Now, obtain the PGP Public Key:

```shell
gpg --armor --export <GPG_KEY_ID>
```

Copy the displayed GPG Public key.

### Step 4: Add GPG Key to Your Account

For Git repositories, the steps are essentially the same. Log in to your account, navigate to the GPG section, and paste the GPG Key.

### Step 5: Verify Your Public GPG Key

In the same section, find a "Verify" button. Copy the provided command line, paste it into your terminal, copy the output, and paste it back into the verification section. Your GPG Key should now be verified.

### Step 6: TTY Session

Before proceeding, ensure the active session can use the GPG key, Add into `~/.zshrc`:

```shell
export GPG_TTY=$(tty)
```

### Step 7: Git Configuration Setup

Configure Git to use your GPG key:

```shell
git config --global user.signingkey <GPG_KEY_ID>
git config --global commit.gpgSign true
```

Replace `<GPG KEY ID>` with your actual GPG key ID.

### Step 8: Commit a Message

When committing, Git will prompt for the passphrase associated with your GPG key, adding an extra layer of security:

```shell
git commit -S -m "commit message"
```

Ensure this commit is made with the corresponding Public Key.

## References

- [7.4 Git Tools - Signing Your Work](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)
- [Unable to generate a key with GnuPG (agent_genkey failed: No such file or directory)](https://superuser.com/questions/1660466/unable-to-generate-a-key-with-gnupg-agent-genkey-failed-no-such-file-or-direct)
- [Adding a GPG key to your account](https://docs.codeberg.org/security/gpg-key/)
- [gnupg2: gpg: public key decryption failed: Inappropriate ioctl for device #14737](https://github.com/Homebrew/homebrew-core/issues/14737)
- [GnuPG](https://wiki.archlinux.org/title/GnuPG)

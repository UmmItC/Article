---
author: "UmmIt"
title: "Git: Configurations Settings"
tags: ["Git"]
date: 2023-12-27T02:21:38+08:00
thumbnail: https://4.bp.blogspot.com/-4VNeYSOCGAY/WgqxG2nifTI/AAAAAAAAbP0/JjAPupBXh6wa6_gHCIui5xp9QM1q77kDgCLcBGAs/s1600/git-goodness.gif
---

## Introduction 

When working with Git, managing configurations is crucial for a seamless development experience. This guide explores setting up local and global configurations and provides a list of configurations for your reference.

## Global Configuration

Global configurations apply to all repositories on your machine. They are useful for providing default identities for repositories without a local configuration. Set up global configurations with the following commands:

```shell
git config --global user.name "Username"
git config --global user.email "email@email.com"
```

Replace `Username` and `email@email.com` with the desired global username and email address.

## Local Configuration

Local configurations are specific to a particular Git repository. To set up a local configuration for a repository, navigate to the repository's directory and run the following commands:

```shell
git config --local user.name "Username"
git config --local user.email "email@email.com"
```

Replace `Username` and `email@email.com` with the desired local username and email address for the repository.

### Verify Configurations

To confirm your configurations, you can use the following commands:

```shell
git config --local --get user.name
git config --local --get user.email
```

Use `--global` instead of `--local` to check the global configurations:

```shell
git config --global --get user.name
git config --global --get user.email
```

### Full Config List

To view the complete list of configurations, use:

```shell
git config --list
```

This command displays all configurations, including user name, email, aliases, and other settings.

## Conclusion

Now, when you work on a specific repository, Git will use the local configuration for that repository. For other repositories or situations where a local configuration is not available, Git will fall back to the global configuration.

This setup is handy when contributing to projects with different identities. Adjust the local configurations for each repository, and global configurations will serve as the default for repositories without a specific local configuration.

By following these steps and referring to the full config list, you can easily manage and switch between different identities when working on multiple Git repositories.

## Reference

- [How can I config two different git repo with different credentials in one system?](https://stackoverflow.com/questions/43118543/how-can-i-config-two-different-git-repo-with-different-credentials-in-one-system/)

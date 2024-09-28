---
author: "UmmIt"
title: "How to install Copilot on Neovim, and about the free trial for students"
description: "The guide to install Copilot on Neovim, The AI coder helper made by GitHub."
tags: ["Neovim", "Copilot", "NvChad"]
date: 2024-09-11T19:26:13+0800
---

## Copilot Overview

GitHub Copilot is an AI-powered code completion tool that helps you write code faster and with less effort. It is available as a plugin for Neovim, which makes it easy to integrate into your existing workflow.

Basically, Copilot uses machine learning models to suggest code completions based on the context of your code. It can help you write functions, classes, and even entire code blocks with just a tab press.

## Supported IDEs

The Copilot offers neovim, vscode and other IDEs. But, in this guide, we will focus on how to install Copilot on Neovim.

### Getting GitHub Copilot for Free

GitHub Copilot isn't free, but you can access a 14-day free trial. If you're a student, you can get GitHub Copilot at no cost through the GitHub Student Pack.

The GitHub Student Pack provides various benefits, including free access to GitHub Copilot. To take advantage of this offer, follow these steps:

1. Make sure you have a valid student email address and billing information that matches your student name.
2. Verify the student email address with your school.
3. Visit the [GitHub Student Pack page](https://education.github.com/pack) to apply. You will need to verify your student status with an identity card or other official documentation.

Once your application is approved, you'll receive access to GitHub Copilot and other benefits included in the Student Pack.

In summary, to get GitHub Copilot for free through the GitHub Student Pack, you need:

1. A student email address
2. Billing information matching your student name
3. now apply the GitHub student pack.

## Installation steps

Lets say you already have Neovim and Nvchad installed, and the github account already have permission to Copilot and you want to install Copilot on it. Here are the steps to do that:

### 1. Install the Copilot plugin

add the following line to your `~/.config/nvim/lua/plugins/init.lua`, which is the plugin manager:

```lua
{
    "github/copilot.vim",
}
```

Now, quit the neovim and reopen it again to begin the installation process, it will automatically install the plugin.

After the installation is complete, back to `init.lua` and add the following line to enable the plugin:

```lua
lazy = false,
```

To load the plugin immediately, set the `lazy` to false. and your `init.lua` should look like this:

```lua
{
    "github/copilot.vim",
    lazy = false,
}
```

### 2. Setup Copilot

Now, back to neovim and you can start setting up the Copilot.

```shell
:Copilot setup
```

The Copilot will ask you to login to your GitHub account. So it's required to login to your GitHub account to authenticate the Copilot, and so, the copilot use the web browser to authenticate.

in the neovim, you will see the message like this:

```
First copy your one-time code: XXXX-XXXX
Press ENTER to open Github in your browser
Opened https://github.com/login/device
Waiting (could take up to 10 seconds)
```

Press enter to open the browser and paste the one-time code to authenticate the Copilot. just follow the instruction to proceed the authentication.

After the authentication is complete, You copilot is ready to use :)

### 3. Keymaps

Since the NvChad might have a keymaps that conflict with the Copilot (like auto-complete). Now, change the keymaps by adding the following line to your `~/.config/nvim/lua/keymaps/init.lua`, and here's my use case:

```lua
{
    "github/copilot.vim",
    lazy = false,
    config = function()
      vim.g.copilot_no_tab_map = true
      vim.api.nvim_set_keymap("i", "<Tab>", "copilot#Accept()", { expr = true, silent = true })
    end,
  },
```

The above code will disable the default tab mapping and use `Tab` to accept the copilot suggestions :)

### 4. Usage

Try on your project type something like `function` and wait for the suggestions. then press `Tab` to accept the suggestions.

### Conclusion

Really mind-blowing how the Copilot can help you write code faster and with less effort. It's like having a pair programmer that can help you write code ...

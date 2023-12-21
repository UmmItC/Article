---
title: "Setting Up a Minecraft Server on VPS: A Comprehensive Guide"
author: "UmmIt"
tags: ["Games", "Minecraft", "VPS", "Self-host"]
date: 2023-12-22T02:20:10+0800
---


## Introduction

Running a Minecraft server on a VPS (Virtual Private Server) allows you to create a shared gaming experience for you and your friends. This guide covers the step-by-step process of setting up a Minecraft server on a VPS, focusing on key tools and configurations.

### Prerequisites:

- VPS Server with a minimum of 2GB RAM.
- SSH (Secure Shell) connection tool.
- Optional: Filezilla or SCP for file transfers.
- Java Edition server for Minecraft.
- Screen for running the server in the background.

## Step 1: Install SSH Connection Tool

Ensure that you have an SSH connection tool installed. Most Linux systems come with an SSH connection tool by default. If not, install it using the appropriate command for your system.

**Debian:**
```bash
sudo apt-get install ssh -y
```

**Arch:**
```bash
sudo pacman -S ssh
```

**Fedora:**
```bash
sudo dnf install ssh
```

## Step 2: Log in to the SSH Server

Use the IP address of your VPS server to log in using SSH. Replace `your_server_Ip` with the actual IP address.

```bash
ssh root@your_server_Ip
```

## Step 3: Accept Port 22

Some VPS providers may deny port 22 by default. To prevent login issues in the future, accept connections on Port 22:

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## Step 4: Update Package

Before installation, update the system's package information:

```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt dist-upgrade -y
```

## Step 5: Install Java JDK

Install OpenJDK version 17, suitable for running Minecraft servers:

```bash
sudo apt install openjdk-17-jre-headless -y
```

## Step 6: Install Screen

Install Screen to run the server in the background:

```bash
sudo apt install screen -y
```

## Step 7: Download the Latest Minecraft Server

Download the latest Minecraft server file using `wget` or from the official website:

```bash
wget https://launcher.mojang.com/v1/objects/e00c4052dac1d59a1188b2aa9d5a87113aaf1122/server.jar
```

## Step 8: Set Up Firewall

Use `ufw` to enable and configure the firewall:

```bash
ufw enable
sudo ufw allow 25565
```

## Step 9: Start the Server

Run the Minecraft server with the following command:

```bash
java -Xms1024M -Xmx1024M -jar server.jar nogui
```

## Step 10: Server Error

After the first run, you may encounter errors related to missing files. This is normal. The next steps will generate and modify the required files.

```bash
Starting net.minecraft.server.Main
[14:01:39] [ServerMain/INFO]: Building unoptimized datafixer
[14:01:42] [ServerMain/ERROR]: Failed to load properties from file: server.properties
[14:01:42] [ServerMain/WARN]: Failed to load eula.txt
[14:01:42] [ServerMain/INFO]: You need to agree to the EULA in order to run the server. Go to eula.txt for more info.
```

## Step 11: Modify eula.txt File

Edit `eula.txt` to change `eula=false` to `eula=true`:

```bash
nano eula.txt
```

## Step 12: Modify server.properties File

Edit `server.properties` to configure your server settings:

```bash
nano server.properties
```

Now you made the following changes, For instances:

```bash
rcon.port=25575
rcon.password=hi
enable-rcon=true
```

## Step 13: Use Screen to Keep the Server Running

Use `screen` to keep the server running in the background:

```bash
screen
java -xms1024m -xmx2048m -jar Server.jar NOGUI
```

## Final Step: Enter Minecraft and Join the Server

1. Open Minecraft and go to "multiplayer game."
2. Add your server's IP and click "finish."
3. Click on your server and join. Enjoy the game!

![gameplay](./featured.png)

## Conclusion

Setting up a Minecraft server on a VPS provides a shared gaming environment for you and your friends. Following these steps ensures a smooth installation and configuration process. Enjoy your Minecraft adventure!

## References:

- [Shells Tutorial](https://www.shells.com/l/en-s/tutorial/0-guide-nstalling-A-linecraft-linux-ubuntu)
- [DigitalOcean Tutorial](https://www.digitalocean.com/community/tutorials/how-create-A--mincraft--ubuntu-20-04-tutorials)
- [Minecraft Server Error Fix](https://www.reddit.com/r/minecraft/comments/nv92fg/error_while_launcher_a_117_minecraft_server_fix/)

## File Download Websites:

- [Minecraft Versions](https://mcversions.net/)
- [Official Minecraft Server Download](https://www.minecraft.net/en-s/download/server)

## Author's Note

I am not a Minecraft player, and I only started playing it last month. I have previously hosted a CS1.6 Server, and I thought it might be similar, but I encountered various problems while writing this article. The main issue was that I couldn't run Minecraft due to insufficient RAM. I initially tried using 512MB of RAM, but it wasn't sufficient. Realizing the RAM was inadequate, I attempted again, this time upgrading to 1GB of RAM, but it still didn't work. Although the error messages were minor, a new error occurred after the last step. Finally, I used 2GB of RAM, and there were no more errors.

Minecraft really resource-intensive and expensive.

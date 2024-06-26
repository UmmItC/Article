---
author: "UmmIt"
title: "The Ultimate Guide to Setting Up a Complete Counter-Strike 1.6: From Zero to Zombie Plague Server, Play with yapb bot and Public the server on the Internet!"
description: "Learn how to set up a Counter Strike 1.6 server on an Ubuntu Virtual Private Server (VPS) with this comprehensive guide, Also learn how to install yapb bot."
tags: ["Games", "Counter-Strike 1.6"]
date: 2021-12-11T00:00:00+0800
thumbnail: https://1.bp.blogspot.com/-a7CfYOytZAU/X-dvdiiERSI/AAAAAAAAGgc/BMoWOxVPdL8l-iOcrGzbtVHqkzbbMgwwACLcBGAsYHQ/s1366/Untitled-1-1.jpg
lastmod: 2024-01-08T12:38:17+0800
---

## Why Choose a VPS for Hosting?

If you're considering hosting your own Counter-Strike 1.6 (CS 1.6) server and welcoming players to join your game world, opting for a Virtual Private Server (VPS) is a wise decision. A VPS offers various advantages, such as enhanced security and stability, making it an optimal choice for setting up your CS 1.6 server. This guide will provide you with step-by-step instructions on how to create and manage a CS 1.6 server on a Linux-based VPS running Ubuntu.

## Step 1: Renting a VPS Server

So, you've decided to embark on the journey of setting up your own CS 1.6 server, complete with all the exciting mods and features. The first step on this exciting adventure is to secure a Virtual Private Server (VPS) that will become the foundation of your gaming paradise. In this part of the guide, we'll explore the process of renting a VPS server, ensuring you have the necessary resources to create an unforgettable CS 1.6 experience.

### Why Opt for a VPS Server?

Before we dive into the intricacies of VPS server rental, let's briefly touch on why a VPS is the preferred choice for hosting your CS 1.6 server. A VPS offers you a dedicated virtual environment, providing the resources and control necessary to create a seamless gaming experience. With the flexibility to choose your server's specifications and the ability to install custom software, a VPS is the perfect platform for your CS 1.6 ambitions.

### Exploring VPS Providers

To begin your journey, you'll need to choose a VPS provider that aligns with your preferences and requirements. There are several reputable options to consider, each offering different pricing plans, server locations, and features. Here are a few noteworthy providers to explore:

- **AWS Lightsail:** Amazon Web Services' (AWS) Lightsail offers an intuitive platform for launching and managing VPS instances. With a variety of pre-configured images and easy scaling options, Lightsail is an excellent choice for beginners and experienced users alike.

- **DigitalOcean:** Known for its user-friendly interface and straightforward pricing, DigitalOcean provides a range of VPS solutions tailored to different needs. Whether you're a gaming enthusiast or a developer, DigitalOcean has options to suit your requirements.

- **Google Cloud:** Google Cloud's Compute Engine offers high-performance virtual machines that can handle the demands of a CS 1.6 server. With advanced networking features and a global network of data centers, Google Cloud provides a solid foundation for your gaming server.

- **Linode:** Linode is renowned for its reliability and competitive pricing. With a focus on simplicity and performance, Linode's VPS offerings are designed to meet the needs of various projects, including game servers.

## Step 2: Logging into Your VPS Server

With your VPS server in hand, it's time to roll up your sleeves and embark on the journey of creating your CS 1.6 gaming paradise. While we won't delve into the intricacies of VPS security (we're here to focus on the gaming action!), let's kick things off by getting you logged into your server.

### Preparing for Server Access

Before we dive into the thrilling world of CS 1.6 setup, you'll need a secure way to access your VPS. This is where SSH (Secure Shell) comes into play. SSH allows you to connect to your server over a secure channel, ensuring that your actions are encrypted and protected from prying eyes.

Your VPS provider will have detailed documentation on how to access your server using SSH. This documentation will include crucial information like the server's IP address, username, and hostname. Make sure to have this information at hand before proceeding.

### Logging In with SSH

Now that you're armed with the necessary information, let's get you logged into your server using SSH. Here's how it's done:

1. **Open a Terminal:** On your local machine, open a terminal. If you're using a Windows machine, you can use tools like PuTTY or Windows Subsystem for Linux (WSL) to access SSH.

2. **SSH Command:** In the terminal, use the SSH command along with the provided IP address, username, and hostname to initiate the connection. For example:

   ```shell
   ssh root@<ip> -v
   ```

   Replace `<ip>` with the actual IP address of your VPS server. The `-v` flag adds verbosity, giving you more information about the connection process.

3. **Verify Connection:** After entering the command, you'll be prompted to verify the authenticity of the server's fingerprint. This is a security measure to ensure you're connecting to the correct server. Type `yes` to continue.

4. **Enter Password:** If this is your first time connecting, you'll be prompted to enter the password associated with the specified username (usually `root`). Note that the password won't be visible as you type it.

5. **Welcome to Your VPS:** Congratulations! If all goes well, you should now be logged into your VPS server. You'll see a command prompt indicating that you're ready to start your CS 1.6 adventure.

And there you have it! You've successfully logged into your VPS server using SSH, setting the stage for the exciting journey ahead. In the upcoming parts of this guide, we'll delve into the installation of essential tools and the step-by-step process of setting up your CS 1.6 server, complete with mods and features. Prepare to become the master of your CS 1.6 domain and let the gaming fun begin!

## Step 3: Updating, Upgrading, and Port 22 Access: Laying the Foundation

Now that you're logged into your VPS server, it's time to ensure that your system is up to date and that you have the necessary port access to maintain a seamless connection. In this segment, we'll be using the reliable Ubuntu distribution to guide us through the process. Ubuntu's stability and performance make it an excellent choice for hosting game servers.

### Up to date Your System

Before we get into the nitty-gritty of gaming, let's make sure your system is fresh and up to date. Run the following commands to update, upgrade, and perform a distribution upgrade on your system:

```shell
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
```

### Allowing Port 22 Access

Port 22 is the default port for SSH connections, allowing you to securely access your server. To ensure that you don't lock yourself out of your server on subsequent logins, you need to allow traffic on port 22.

Enter the following command to enable SSH access:

```shell
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

With this rule in place, you're granting permission for SSH traffic to enter through port 22, which means you can log in without any obstacles.

## Step 4: Allowing Server Ports with UFW

Imagine ports as the doorways to your server – they determine what type of traffic is allowed in and out. In this step, we're going to ensure that the necessary port for your CS 1.6 server is open and ready to receive players. Think of it as inviting gamers into your digital arena.

Previously, we used `iptables` to manage ports, but for simplicity's sake, we're introducing `ufw` (Uncomplicated Firewall), a user-friendly alternative. We're going to allow port 27015 this time, which is a common choice for CS 1.6 servers.

### Installing and Enabling UFW

First things first, let's install `ufw` and enable it to ensure it starts up automatically after system boot:

```shell
sudo apt-get install ufw -y
sudo ufw enable
```

### Allowing Port 27015

Now, let's grant access to port 27015, which is a common UDP port used by CS 1.6 servers:

```shell
sudo ufw allow 27015/udp
```

## Step 5: Installing SteamCMD

SteamCMD is for Generate game server files. It allows you to download and update game server files from Steam's content distribution system. In this step, we'll install SteamCMD, which is essential for setting up and maintaining your CS 1.6 server.

To get started, follow these instructions (assuming you're using Ubuntu):

1. **Adding the Multiverse Repository**:
   
   Multiverse is an Ubuntu repository that contains software packages that aren't officially supported by Canonical but are still maintained by the community. To add the Multiverse repository, run:

   ```shell
   sudo add-apt-repository multiverse
   ```

2. **Installing Required Packages**:
   
   Before installing SteamCMD, let's make sure your system is ready. Install the necessary packages using the following commands:

   ```shell
   sudo apt install software-properties-common -y
   sudo dpkg --add-architecture i386
   sudo apt update -y
   ```

3. **Installing SteamCMD**:
   
   With the prerequisites in place, you're ready to install SteamCMD:

   ```shell
   sudo apt install lib32gcc-s1 -y
   sudo apt install steamcmd -y
   ```

## Step 6: Installing Game Server Required Files

Your CS 1.6 server needs the necessary game files to function properly. In this step, we'll use SteamCMD to download and install these files.

1. **Open SteamCMD**:
   
   Launch SteamCMD by entering the following command in your terminal:

   ```shell
   steamcmd
   ```

2. **Login with anonymous**:
   
   Once SteamCMD is running, log in as an anonymous user with:

   ```shell
   login anonymous
   ```

3. **Install CS 1.6 Server Files**:
   
   Now that you're logged in, it's time to download and install the CS 1.6 server files. Enter the following commands:

   ```shell
   app_update 90 validate
   ```

   The number `90` corresponds to the CS 1.6 game ID. The `validate` parameter ensures that the files are validated and correctly installed. 
   >Notes: You might encounter an error the first time you try to run `app_update 90 validate`. Don't worry, this is a common issue with SteamCMD. Simply try the `app_update 90 validate` command again, and it should work fine.

4. **Exit SteamCMD**:
   
   Once the server files are downloaded and installed, you can exit SteamCMD by typing:

   ```shell
   exit
   ```

## Step 7: Testing the Server

Now, your CS 1.6 server is set up with the basic game files. However, before you can dive into the action, you need to start the server and ensure everything is running as expected. Let's put your server to the test!

1. **Navigate to the Server Directory**:
   
   The CS 1.6 server files are located in the following directory:

   ```shell
   ~/.steam/steamapps/common/Half-Life/
   ```

   Use the `cd` command to navigate to this directory:

   ```shell
   cd ~/.steam/steamapps/common/Half-Life/
   ```

2. **Start the Server**:
   
   With the server files directory as your current location, use the following command to start the CS 1.6 server:

   ```shell
   screen -dmS hlds ./hlds_run -game cstrike +map de_dust2 -port 27015 +maxplayers 32 -insecure
   ```

   Let's break down the components of this command:

   - `-dmS hlds`: This portion of the command sets up a detached screen session named `hlds` for the server process.
   - `./hlds_run`: This command starts the server program.
   - `-game cstrike`: Specifies that the game being run is Counter-Strike 1.6.
   - `+map de_dust2`: Specifies the initial map to load, in this case, `de_dust2.`
   - `-port 27015`: Sets the server's port to 27015.
   - `+maxplayers 32`: Limits the maximum number of players in a game to 32.
   - `-insecure`: Disables Valve Anti-Cheat (VAC) for now.

3. **Access Your Server**:

   Once the server is running, it's time to see the fruits of your labor. Launch Counter-Strike 1.6 on your client machine. In the game's console, press the tilde key (`~`) to open the console and enter the following command:

   ```shell
   connect <your_server_ip>:27015
   ```

   Replace `<your_server_ip>` with the actual IP address of your VPS.

   Alternatively, you can search for your server in the server list within the game and connect from there.

4. **detach and Managing the Server**:

   To leave the server running in the background while you exit the terminal, press `Ctrl+A` followed by `Ctrl+D`. This will detach the screen session without stopping the server.

   If you want to return to the server console at any time, use the following command:

   ```shell
   screen -r hlds
   ```

With your server up and running, you've accomplished a major part of the setup process. In the next steps, we'll dive deeper into configuring your server and adding plugins to make it the ultimate CS 1.6 Zombie Plague experience!

## Step 8: Download Required Files for AMXXModX and Metamod

To enhance your CS 1.6 server with additional features and functionalities, you'll need to install AMXXModX (Advanced Multi-Mod X) and Metamod. These plugins will allow you to create and add custom game modes, plugins, and more. Let's get started by downloading the necessary files

>Notes: Just follow me here, because AMXX are almost never updated. As for the explanation... it's a pity because no one plays 1.6, and the most stable version is 1.82...

1. **Download AMXXModX Base Files**:

   Use the `wget` command to download the base files of AMXXModX version 1.8.2:

   ```shell
   wget https://www.amxmodx.org/release/amxmodx-1.8.2-base-linux.tar.gz
   ```

2. **Download AMXXModX Addon Files**:

   Similarly, download the addon files for AMXXModX version 1.8.2. This step is important for CS 1.6 servers as it provides compatibility with Counter-Strike:

   ```shell
   wget https://www.amxmodx.org/release/amxmodx-1.8.2-cstrike-linux.tar.gz
   ```

3. **Download Metamod Files**:

   Next, let's download Metamod version 1.21.1, which acts as a plugin management system for Half-Life mods like AMXXModX:

   ```shell
   wget https://www.amxmodx.org/release/metamod-1.21.1-am.zip
   ```

## Step 9: Extract and Install AMXXModX and Metamod Files

Now that you have downloaded the necessary files for AMXXModX and Metamod, let's proceed to extract and install them. Follow these steps carefully to ensure a successful installation:

>Attention: Please note here that the order of decompression is `base` > `addon` > `metamod`. <br/>metamod - You can not shuffle the order, but the base must be passed through the addon first

1. **Extract AMXXModX Base Files**:

   First, extract the base files of AMXXModX using the following command:

   ```shell
   tar zxvf amxmodx-1.8.2-base-linux.tar.gz
   ```

2. **Extract AMXXModX Addon Files**:

   Next, extract the addon files for AMXXModX that are specific to Counter-Strike using the command:

   ```shell
   tar zxvf amxmodx-1.8.2-cstrike-linux.tar.gz
   ```

3. **Extract Metamod Files**:

   Unzip the Metamod files using the command:

   ```shell
   unzip metamod-1.21.1-am.zip
   ```

4. **Install AMXXModX**:

   After extracting the files, navigate to the AMXXModX `addons` directory that was created. Use the `cp` command to copy the extracted `addons` directory into the appropriate location for your CS 1.6 server:

   ```shell
   cp -r addons/ ~/.steam/steamapps/common/Half-Life/cstrike/
   ```

## Step 10: Edit and Create Configuration Files

Now, these steps to make the necessary changes and create new configuration files:

1. **Modify liblist.gam**:

   The `liblist.gam` file contains crucial information about the game's libraries. Open the file using the `nano` text editor:

   ```shell
   nano ~/.steam/steamapps/common/Half-Life/cstrike/liblist.gam
   ```

   Inside the file, locate the line that starts with `gamedll_linux` and change it to:

   ```shell
   gamedll_linux `addons/metamod/dlls/metamod.so`
   ```

   After making the change, press `Ctrl + S` to save and then `Ctrl + X` to exit the `nano` editor.

2. **Create plugins.ini**:

   Next, create a new configuration file called `plugins.ini` under the `addons/metamod` directory:

   ```shell
   nano ~/.steam/steamapps/common/Half-Life/cstrike/addons/metamod/plugins.ini
   ```

   In the newly created file, add the following line:

   ```shell
   linux addons/amxmodx/dlls/amxmodx_mm_i386.so
   ```

   Once again, press `Ctrl + S` to save and then `Ctrl + X` to exit the `nano` editor.

## Step 11: Testing Your Server (AMXX Installation Verification)

Before proceeding to the next step of installing Zombie Plague, let's ensure that your game server is up and running with AMXX installed correctly.

1. Restart Your Server:
   Restart your game server using the `screen` command:

   ```shell
   screen -r hlds
   ```

   Press Ctrl+C to turn it off.

2. Start the Server:
   Navigate to the server directory:

   ```shell
   cd ~/.steam/steamapps/common/Half-Life/
   ```

   Initiate the server with the following command:

   ```shell
   screen -dmS hlds ./hlds_run -game cstrike +map de_dust2 -port 27015 +maxplayers 32 -insecure
   ```

3. Verify AMXX and Metamod:
   Access the server console:

   ```shell
   screen -r hlds
   ```

   Type the following commands to see if the plugins are properly loaded:

   ```shell
   meta list # Should be listing 3 plugins
   amxx list # Sould be listing 21 plugins
   ```

   If the results display a list of plugins, you're on the right track.

## Step 12: Installing the Zombie Plague Mod

Prepare to infuse the Zombie Plague mod into your CS 1.6 server. Follow these steps meticulously for a seamless installation:

1. **Obtaining the Zombie Plague Mod:**

   Introduce the captivating zombie apocalypse gameplay of Zombie Plague to CS 1.6. Obtain the necessary mod files from the following links:

   - [Zombie Plague 2014 Version](https://forums.alliedmods.net/showthread.php?s=d63394212992e827c6577de504a895bc&t=164926) (Download the SMA files from here)
   - [Zombie Plague 2008 Version](https://forums.alliedmods.net/showthread.php?s=d63394212992e827c6577de504a895bc&t=72505) (Download the resource files from here)

2. **Downloading the Files:**

   Download the mod files directly to your server using the `wget` command. Execute these commands to fetch the essential resources and plugins:

   ```shell
   wget `https://forums.alliedmods.net/attachment.php?attachmentid=136034&d=1412085945` -O zp_resources.zip
   wget `https://forums.alliedmods.net/attachment.php?s=46bcb4236bef6c971e41f8e763a94c24&attachmentid=28817&d=1216059497` -O zp_plugins.zip
   ```

   These commands retrieve the required files and save them as `zp_resources.zip` and `zp_plugins.zip`.

3. **Unzipping the Files:**

   Create a dedicated folder for extracted files to keep things organized:

   ```shell
   mkdir zp_files
   ```

   Unzip the downloaded files into this new folder using the verbose mode for more detailed output:

   ```shell
   cd zp_files
   unzip -v ../zp_resources.zip
   unzip -v ../zp_plugins.zip
   ```

   This ensures that all files are properly extracted while providing additional information about the extraction process.

4. **Copying Files to the `cstrike/` Directory:**

   Transfer all the extracted files to the `cstrike` directory, where your CS 1.6 server files are located:

   ```shell
   cp -rv * ~/.steam/steamapps/common/Half-Life/cstrike/
   ```

   This command copies all files and directories from the `zp_files` folder to the `cstrike` directory.

5. **Compiling SMA Files:**

   Move to the scripting directory for AMX Mod X:

   ```shell
   cd ~/.steam/steamapps/common/Half-Life/cstrike/addons/amxmodx/scripting
   ```

   This directory houses the scripts for plugins.

   Compile the scripts using the provided compilation script:

   ```shell
   ./compile.sh
   ```

   This command compiles the scripts into executable plugin files.

6. **Copying Compiled Plugin Files:**

   After compiling the scripts, navigate to the `compiled` directory, This is where the compiled plugin files are stored:

   ```shell
   cd compiled
   ```

   Copy the compiled AMXX files to the plugins directory:

   ```shell
   cp -v *.amxx ../../plugins/
   ```

   This command moves the compiled plugin files from the `compiled` directory to the correct location within the `plugins` directory, allowing AMX Mod X to properly load and utilize them.

## Step 13: Testing your Server (Zombie Plague Installation Verification)

Now that you've successfully installed the Zombie Plague mod on your CS 1.6 server, it's time to verify the installation and ensure that everything is working as expected.

1. **Restart Your Server:**
   Begin by restarting your CS 1.6 server using the `screen` command. This step ensures that any previous server instances are closed and prepares the environment for the Zombie Plague mod.

   ```shell
   screen -r hlds
   ```

   If the server is currently running, press Ctrl+C to halt it.

2. **Start the Server with Zombie Plague Mod:**
   To enable the Zombie Plague mod on your server, navigate to the server directory using the following command:

   ```shell
   cd ~/.steam/steamapps/common/Half-Life/
   ```

   Now, initiate the server with the Zombie Plague mod activated by executing the following command:

   ```shell
   screen -dmS hlds ./hlds_run -game cstrike +map de_dust2 -port 27015 +maxplayers 32 -insecure
   ```

   To access the server console and monitor its activity, use the command:

   ```shell
   screen -r hlds
   ```

   If you wish to temporarily exit the screen session without stopping the server, press Ctrl+A and then press D.

3. **Check if Zombie Plague Mod is Active:**
   Return to the screen session where your server is running:

   ```shell
   screen -r hlds
   ```

   Confirm that the Zombie Plague mod has been successfully integrated into the server by using the following command:

   ```shell
   amxx list
   ```

   This command displays a list of loaded plugins, including Zombie Plague if it's properly configured. Verify that Zombie Plague is among the listed plugins, indicating its successful installation.

4. **Join Your Zombie Plague Server:**
   It's time to experience the thrill of the Zombie Plague gameplay mode you've just installed. Launch Counter-Strike 1.6 and access the game's console by pressing the backtick (`) key.

   **Way1: Command-line:** To join your Zombie Plague server, type the following command in the console, replacing `your_server_ip` with your server's IP address and `port` with the configured port number:

   ```shell
   connect your_server_ip:port
   ```

   **Way2: Gui:** Alternatively, you can utilize the GUI method to connect by adding your server's IP to the server list within the game interface.

   When you successfully join the server, you should be greeted with a gameplay experience similar to the following image:

   ![Zombie Plague Gameplay](./zombie_plague_gameplay.png)

## Step 14: Adding Yapb Bot to Your Zombie Plague Server

Congratulations on successfully installing Zombie Plague on your server! However, playing alone on your server might not be as exciting as having other players. In this final step, we'll show you how to add Yapb bot to your Zombie Plague server using the bot's source code.

1. **Install Required Packages:**
   First, navigate to your home directory and install the necessary packages:

   ```shell
   cd ~
   sudo apt update
   sudo apt install build-essential git clang python3 gcc-multilib g++-multilib meson ninja
   ```

   These packages provide the tools and libraries needed to compile and configure the Yapb bot.

2. **Clone the Yapb Repository:**
   Clone the Yapb bot's repository, including its submodules:

   ```shell
   git clone --recursive https://github.com/yapb/yapb
   ```

3. **Navigate to the Yapb Directory:**
   Move into the Yapb bot's directory:

   ```shell
   cd yapb
   ```

4. **Configure the Project Using Meson:**
   Set up the project configuration using Meson:

   ```shell
   meson setup build
   ```

5. **Compile the .so Library:**
   Compile the bot's .so library:

   ```shell
   meson compile -C build
   ```

6. **Copy Compiled Files to addons/yapb/bin/:**
   Copy the compiled `yapb.so` library to the `addons/yapb/bin/` directory:

   ```shell
   mkdir -pv ~/.steam/steamapps/common/Half-Life/cstrike/addons/yapb/bin/
   cp -v build/yapb/yapb.so ~/.steam/steamapps/common/Half-Life/cstrike/addons/yapb/bin/
   ```

7. **Modify plugins.ini:**
   Open the `plugins.ini` file using a text editor:

   ```shell
   nano ~/.steam/steamapps/common/Half-Life/cstrike/addons/metamod/plugins.ini
   ```

   Append the following entry to the file:

   ```shell
   linux addons/yapb/bin/yapb.so
   ```

8. **Save and Apply Changes:**
   Save the changes you made to the `plugins.ini` file. This entry ensures that the Yapb bot will be loaded and integrated into your Zombie Plague server when it starts.

## HTTP Download vs. Server Download: Which Is Faster?

When setting up FastDL for your Counter-Strike 1.6 server, you have the option to allow players to download directly from an HTTP source or from your game server. It's important to consider the speed and efficiency of these methods.

**HTTP Download:** 

   - FastDL resources accessed via HTTP URLs are typically faster for players to download. Players connect to the FastDL server directly, which can result in quicker download times, especially for large files like maps and custom sounds.

**Server Download:** 

   - When custom content is downloaded directly from your game server, it might take longer, especially if multiple players are simultaneously connecting and downloading resources. This method can lead to slower download speeds and potential delays for players.

To ensure the best gaming experience for your players, it's recommended to set up FastDL with HTTP download support. This way, your custom content will be readily available for players to download at optimal speeds.

## Installing a Web Server for FastDL

To host your FastDL resources via HTTP, you'll need a web server. You can choose between popular options like Apache or Nginx. Here's how to install them

> Notes: Just choice one for your http server. If you want to be more customizable later on, I recommend using nginx.

**Installing Apache:**

1. Open your terminal.

2. Run the following command to install Apache:

   ```shell
   sudo apt-get install apache2 -y
   ```

3. **Enabling Apache Service:** After installing Apache, you'll need to enable and start the service. Use the following commands:

   ```shell
   sudo systemctl enable apache2   # Enable Apache service to start on boot
   sudo systemctl start apache2    # Start Apache service
   ```

4. Once installed, Apache's server files are located in the `/var/www/html/` directory.

**Installing Nginx:**

1. Open your terminal.

2. Run the following command to install Nginx:

   ```shell
   sudo apt-get install nginx -y
   ```

3. **Enabling Nginx Service:** Similarly, for Nginx, you'll need to enable and start the service:

```shell
sudo systemctl enable nginx   # Enable Nginx service to start on boot
sudo systemctl start nginx    # Start Nginx service
```

4. Once installed, Nginx's server files are also located in the `/var/www/html/` directory.

Enabling the service ensures that your web server will automatically start when your system boots up, making your FastDL resources accessible to players connecting to your Counter-Strike 1.6 server.

## Configuring Your Server for FastDL

To enable FastDL on your Counter-Strike 1.6 server, you'll need to configure the `server.cfg` file. Here are the steps:

1. Locate your `server.cfg` file. You can usually find it in the following directory:
   ```shell
   ~/.steam/steamapps/common/Half-Life/cstrike/server.cfg
   ```

2. Open `server.cfg` using a text editor of your choice.

3. Add the following line to the file, replacing ``http://your-fastdl-server.com/`` with the actual URL to your FastDL server directory:

   ```shell
   sv_downloadurl `http://your-fastdl-server.com/`
   ```

   This line tells your Counter-Strike 1.6 server to use the specified URL for downloading custom content.

4. Save the changes to the `server.cfg` file.

5. Put all your resource files in /var/html/www/. Be sure to match the location, otherwise it will not be downloaded.

6. Restart your server.

Now, when players connect to your Counter-Strike 1.6 server, the game will automatically fetch custom content from your FastDL server, enhancing their gaming experience.

### Additionally

you can access your FastDL server's files via a web browser using the following format:

   ```shell
   http://your-fastdl-server.com/
   ```

   This URL will allow you to manage and organize your FastDL resources for your server.

## **Important Note for Counter-Strike 1.6 Server Owners**

If you're a Counter-Strike 1.6 server owner considering the use of a custom domain through Cloudflare, there's a crucial aspect to keep in mind. Counter-Strike 1.6 does not support HTTPS (TLS) connections, which is essential to understand when working with custom domains and Cloudflare.

Here's what you need to know:

**1. HTTPS (TLS) Not Supported:** Counter-Strike 1.6 relies on plain HTTP connections, not HTTPS (TLS). This means that when players connect to your server, it happens over an unencrypted HTTP connection.

**2. Cloudflare's Proxy Service:** Cloudflare provides a proxy service that, by default, routes traffic through its servers. This can enable HTTPS for your domain, but it also means that all traffic is converted to HTTPS.

**3. Non-Proxy (DNS Only) Status:** To ensure that players can connect to your Counter-Strike 1.6 server, you must set the custom domain to `DNS Only` status in Cloudflare. This configuration will route the traffic directly to your server without going through Cloudflare's proxy servers. This is crucial because, as mentioned, CS 1.6 doesn't support HTTPS, and forcing HTTPS through Cloudflare will result in connection issues.

**4. Maintaining Server Connectivity:** By setting your custom domain to `DNS Only` status in Cloudflare, you ensure that players can connect to your Counter-Strike 1.6 server without any encryption-related problems. This maintains the compatibility required for a seamless gaming experience.

It's essential to follow these steps to guarantee that your custom domain and Cloudflare setup don't interfere with the connectivity of your Counter-Strike 1.6 server. By prioritizing non-proxy (DNS only) status, you'll ensure that players can effortlessly join your server without encountering any obstacles related to HTTPS compatibility.

### DNS Record Configuration for Your Domain

When configuring the DNS record for your domain in Cloudflare, it's crucial to set it to `DNS Only.` This ensures that your domain does not go through Cloudflare's proxy service, allowing you to maintain compatibility with services like Counter-Strike 1.6 that rely on plain HTTP connections.

**DNS Only (No Proxy):** For your domain's DNS record, select the option that specifies `DNS Only` or `No Proxy.` This configuration ensures that traffic to your domain goes directly to your server without being routed through Cloudflare's proxy servers.

While your DNS record remains `DNS Only,` you can still use HTTPS within your HTTP server for secure connections. This means that when someone accesses your website using HTTPS in their browser, your server can deliver content securely over HTTPS. However, it's important to note that your game server, like Counter-Strike 1.6, will continue to use HTTP for resource downloads.

By configuring your DNS record in this way, you strike a balance between maintaining compatibility with older services like Counter-Strike 1.6 and providing secure HTTPS access to your website for modern browsers. This setup allows you to ensure smooth connectivity for all users while keeping your resources accessible via HTTP for older systems.

## Conclusion

Congratulations! You've successfully transformed your Counter-Strike 1.6 server into an exciting Zombie Plague server complete with AI-controlled Yapb bots, Also using fastdl for HTTP download your server resources.

Now, you can access your server with your custom domain, such as `cs.yourserver.io` or `download.yourserver.io.` Happy gaming!

## References

- [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD#Debian-Based_Distributions_.28Ubuntu.2C_Mint.2C_etc..29)
- [Zombie Plague Mod 5.0](https://forums.alliedmods.net/showthread.php?s=d63394212992e827c6577de504a895bc&t=72505)
- [ZP 5.0 Betas/Updates](https://forums.alliedmods.net/showthread.php?s=d63394212992e827c6577de504a895bc&t=164926)
- [Official YaPB Documentation](https://yapb.readthedocs.io/en/latest/building.html)
- [Github - YaPB](https://github.com/yapb/yapb)

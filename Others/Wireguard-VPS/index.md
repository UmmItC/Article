---
author: "UmmIt"
title: "How to Build Your Own WireGuard VPN Server and Connect from Anywhere!"
description: "Learn how to set up your very own WireGuard VPN server and securely connect from anywhere!"
tags: ["Wireguard", "VPN", "Server", "GNU/Linux", "Android"]
date: 2024-11-25T02:31:28+0800
thumbnail: https://cdn.mos.cms.futurecdn.net/Vo6wb23SMpP2HF3QQmAAmc.jpg
lastmod: 2024-11-25T13:05:22+0800
---

## Introduction

WireGuard is a modern VPN protocol that’s fast, lightweight, and secure. Designed to outperform traditional tunneling protocols like IPsec and OpenVPN, it uses UDP to send traffic, making it ideal for high-performance connections.

In this tutorial, we’ll walk through how to set up your very own WireGuard VPN server and client. Once you’re done, you’ll have a private, secure, and blazing-fast VPN to use wherever you go. Let’s dive in!

### Prerequisites

A GNU/Linux server where you’ll set up the WireGuard VPN.

A client device (e.g., your phone or laptop) with the WireGuard app installed to connect to your server.

### Step 1: Install Wireguard

Let’s get started by installing WireGuard on your server. Before installation, make sure your server is updated and rebooted—this ensures everything runs smoothly since WireGuard includes a kernel module.

```bash
sudo apt update -y && sudo apt upgrade -y && sudo apt dist-upgrade -y
sudo apt install wireguard
sudo reboot
```

### Step 2: Login to root user

To avoid typing sudo repeatedly, switch to the root user. This is handy because all WireGuard configuration files are located in `/etc/wireguard`, and root access is required to edit them.

```bash
sudo su
```

### Step 3: Generate the Wireguard Nessessary Keys

Time to generate the keys for the server and client. WireGuard uses these keys for secure communication. Here’s what we need:

- Sever Private Key, Public Key
- Client Private Key, Public Key
- Pre-Shared Key

While you can paste keys directly into configuration files, saving them in files is more organized and makes them easier to reuse.

Now, to generate the server private and public keys, run the following commands:

```bash
cd /etc/wireguard

wg genkey | tee server_privatekey | wg pubkey > server_publickey
```

Now, generate the client private and public keys.

```bash
wg genkey | tee client_privatekey | wg pubkey > client_publickey
```

Finally, generate the pre-shared key.

```bash
wg genkey > presharedkey
```

So the total files should have 5 files in the `/etc/wireguard` directory.

```bash
root@wg-o:/etc/wireguard# ls -l
total 20
-rw-r--r-- 1 root root  45 Nov 24 17:20 client_privatekey
-rw-r--r-- 1 root root  45 Nov 24 17:20 client_publickey
-rw-r--r-- 1 root root  45 Nov 24 17:17 server_privatekey
-rw-r--r-- 1 root root  45 Nov 24 17:17 server_publickey
-rw-r--r-- 1 root root  45 Nov 24 17:17 presharedkey
```

### Step 4: Configure the Wireguard Server

Create a configuration file called `wg0.conf` in `/etc/wireguard`. This file defines the WireGuard server settings.

```bash
vim /etc/wireguard/wg0.conf
```

Add the following configuration, replacing placeholders with your keys and values:

```bash
root@wg-o:/etc/wireguard# cat wg0.conf 
[Interface]

# Address is the IP address that you want to assign.
Address = 10.66.66.1/24

# ListenPort is the port that you want to listen to.
ListenPort = <PORT NUMBER>

# PrivateKey is the server private key.
PrivateKey = <SERVER PRIVATE KEY>

# Set the port number of wireguard to 51820
# the interface name is wg0
# if not sure, use the command 'ip -c a' to check the interface name
PostUp = iptables -I INPUT -p udp --dport 51820 -j ACCEPT
PostUp = iptables -I FORWARD -i eth0 -o wg0 -j ACCEPT
PostUp = iptables -I FORWARD -i wg0 -j ACCEPT
PostUp = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D INPUT -p udp --dport 51820 -j ACCEPT
PostDown = iptables -D FORWARD -i eth0 -o wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

### Client 1
[Peer]

# PublicKey is the client public key.
PublicKey = <CLIENT PUBLIC KEY>

# PresharedKey is the pre-shared key.
PresharedKey = <PRESHARED KEY>

# Set this IP you want to assign to the client
AllowedIPs = 10.66.66.2/32
```

### Step 5: Configure the Wireguard client

On the client device, create a configuration file. You can save it anywhere, such as `/root/client1.conf`.

Let say create the file on `/root/client1.conf`

```bash
[Interface]

# PrivateKey is the client private key.
PrivateKey = <CLIENT PRIVATE KEY>

# Address is the IP address that you want to assign.
Address = 10.66.66.2/32

# DNS is the DNS server that you want to assign.
# Optional, if you want to use the DNS server.
DNS = 1.1.1.1,1.0.0.1

[Peer]

# PublicKey is the server public key.
PublicKey = <SERVER PUBLIC KEY>

# PresharedKey is the pre-shared key.
PresharedKey = <PRESHARED KEY>

# Endpoint is the server IP address and port number.
Endpoint = <SERVER IP>:<PORT NUMBER>

# AllowedIPs is the IP address that you want to route.
AllowedIPs = 0.0.0.0/0
```

### Step 6: IP Forwarding

Enable IP forwarding on the server so traffic can flow between the server and client.

```bash
echo "net.ipv4.ip_forward=1" | tee -a /etc/sysctl.conf
sysctl -p
```

### Step 7: Start the Wireguard Server

Start the Wireguard server.

>Note: You should got nothing output if the server is started successfully :)

```bash
systemctl enable wg-quick@wg0.service
systemctl start wg-quick@wg0.service
```

### Step 8: Generate the QR Code

Typing client configuration on your phone can be annoying, so let’s generate a QR code instead! First, install `qrencode`:

```bash
apt install qrencode
```

Now, generate the QR code for the client, the read-from is the client configuration file. So that why the path is not matter, you can create the file on everywhere you want.

```bash
qrencode --read-from=/root/client1.conf --type=UTF8
```

And you should get the QR code.

### Step 9: Connect to the Wireguard Server

Install the Wireguard client on the client device, like Android and Scan the QR code to connect to the Wireguard server.

And you should see the packet is sending and receiving on the server :) that mean you are successfully connected to the Wireguard server. and you now can enjoy the VPN service which is provided by your own Wireguard server 🤞🤞🤞

Get the client apk here: [Wireguard Android](https://github.com/WireGuard/wireguard-android)

### Feeling 

Honestly, I research so many article about Wireguard, but those article really bad. I think this version is the best one. Most of the article is missing the keypoint of the targeting the private and public key how to generate and not clearly explain. And missing the PostUp and PostDown configuration.

So I decided to write this article to help those who are looking for the Wireguard VPN Server tutorial. I hope this article can help you to build your own Wireguard VPN Server :)

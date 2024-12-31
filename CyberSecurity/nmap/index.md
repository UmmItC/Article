---
author: "UmmIt"
title: "A Simple Guide to Using Nmap for Network Scanning"
description: "In this comprehensive guide, we will explore how to harness the capabilities of Nmap to check your Network Security."
tags: ["nmap", "Security", "Cyber security", "Network Security", "Port Scanning"]
date: 2021-12-14T17:07:28+0800
thumbnail: https://www.kali.org/tools/nmap/images/nmap-logo.svg
lastmod: 2025-01-01T04:03:00+0800
---

## Introduction

Nmap is a powerful tool for scanning networks. It helps you find open ports, services, and potential security issues in your home network or server. This guide will show you how to use Nmap to check your network security.

## Installing Nmap

To use Nmap, you need to install it. It works on Windows, macOS, and Linux. If you're using Kali Linux, Nmap is already installed. For other Linux systems, you can install it with:

```bash
sudo apt update -y
sudo apt install nmap -y
```

### Scan a single IP

To scan a device on your network, use the following command:

```bash
nmap 192.168.1.1
```

>*This command scans the specified IP address (192.168.1.1) to identify open ports and services running on that device.*

### Scan Multiple IPs

You can scan multiple devices on your network by specifying a range of IP addresses. For example:

```bash
nmap 192.168.1.1-20
```

>*This command scans a range of IP addresses from 192.168.1.1 to 192.168.1.20, allowing you to check multiple devices in one go.*

### Service Version Detection

To detect the versions of services running on open ports, use:

```bash
nmap -sV <target>
```

>*This command uses the `-sV` option to detect the versions of services running on the open ports of the specified target.*

### Scan a Larger Network

You might want to scan a larger network to identify all devices and services. As an useful example of how to known the Range of IP addresses in a network, you can refer to the CIDR notation table :)

| **CIDR Notation** | **IP Address Range**          |
|-------------------|-------------------------------|
| **/24**           | 192.168.0.0 - 192.168.0.255   |
| **/16**           | 192.168.0.0 - 192.168.255.255 |
| **/8**            | 192.0.0.0 - 192.255.255.255   |
| **/0**            | 0.0.0.0 - 255.255.255.255     |

And you can use the following command to scan a larger network:

```bash
nmap 192.168.0.0/24
```

>*This command scans all IP addresses in the specified subnet (192.168.0.0/24), which includes 256 addresses from 192.168.0.1 to 192.168.0.255.*

### Service Detection

To detect services running on open ports, use:

```bash
nmap -Pn -sV <target>
```

>*This command performs a scan on the target without pinging it first (using `-Pn`), while also detecting service versions.*

### Scan Without Ping

```bash
nmap -Pn <target>
```

>*This command scans the target directly without checking if it is online, which can be useful for devices that do not respond to ping requests.*

### Ping Scan (Check Online Hosts)

```bash
nmap -sP <target>
```

>*This command performs a ping scan to identify which hosts are online in the specified range or target.*

### List Scan (Show Hosts Without Scanning)

```bash
nmap -sL <target>
```

>*This command lists all the targets without actually scanning them, which can be useful for generating a list of hosts for further analysis.*

### OS Detection

Nmap also provides OS detection capabilities. To detect the operating system of a target, use `-O` option. Operating system detection involves sending low-level networking packets and certain ICMP requests, which require root or administrator permissions. Also known as TCP/IP fingerprinting for OS scanning.

```bash
nmap -O <target>
```

>*This command attempts to identify the operating system of the target by analyzing the responses to various network packets.*

### TCP SYN Scan with OS Detection

To perform a TCP SYN scan (stealth scan) and attempt to guess the target's operating system, use the `-sS -O` flags:

```bash
sudo nmap -sS -O <target>
```

*This command runs a TCP SYN scan (which is less detectable) and attempts to identify the operating system of the target.*

### TCP Connect Scan

To perform a TCP connect scan, use the `-sT` flag:

```bash
nmap -sT <target>
```

>*This command performs a full TCP connect scan on the target, which establishes a full connection to each port to check if it is open.*

### Scanning for Specific Ports

To scan for specific ports, use the `-p` flag followed by the port numbers. For example, to scan ports 80, 443, and 8080, use:

```bash
nmap -p 80,443,8080 <target>
```

>*This command scans the specified ports (80, 443, and 8080) on the target to check for open services.*

### Scanning range of Ports

To scan a range of ports, use the `-p` flag followed by the range of ports. For example, to scan ports 1 to 100, use:

```bash
nmap -p 1-100 <target>
```

>*This command scans the range of ports from 1 to 100 on the target to check for open services.*

### Scanning All Ports

To scan all ports (1-65535), use the `-p-` flag:

```bash
nmap -p- <target>
```

>*This command scans all ports on the target to check for open services.*

### Combo Options

You can combine multiple options to perform a more comprehensive scan. For example, to perform a SYN scan with version detection and OS detection and scan all ports, use:

```bash
sudo nmap -sS -sV -O <target>
```

## Vulnerability Scanning with Nmap

Nmap can also check for known vulnerabilities using scripts like `nmap-vulners`. The source code can be found at [vulnersCom/nmap-vulners](https://github.com/vulnersCom/nmap-vulners).

### Ensure `nmap-vulners` is Installed

To check if `nmap-vulners` is installed, run the following command, if you see a result like `vulners.nse`, it means that the `nmap-vulners` script is installed and available for use with your Nmap installation:

```bash
ls /usr/share/nmap/scripts | grep vulners
```

If you don't see any results, it's likely that the script isn't installed. You can download the script from the repository and place `*.nse` on `/usr/share/nmap/scripts/`.

### Scanning with `nmap-vulners`

Run the following command to check for vulnerabilities:

```bash
nmap --script vulners <target>
```

>*This command runs Nmap and applies the `nmap-vulners` script to your scan, checking for known vulnerabilities.*

### Specity CVSS Score

You can specify the minimum CVSS score for vulnerabilities to be reported. For example, to only show vulnerabilities with a CVSS score of 7 or higher, use:

```bash
nmap --script vulners --script-args mincvss=7 <target>
```

>*This command runs Nmap with the `nmap-vulners` script and only reports vulnerabilities with a CVSS score of 7 or higher.*

## Conclusion

Nmap is a versatile tool that can help you identify potential security issues in your network. By scanning your network with Nmap, you can find open ports, services, and vulnerabilities that may be exploited by attackers :)

---
author: "UmmIt"
title: "Configuring DNS Over HTTPS (DoH) on your system"
description: "Enhance your online privacy by configuring DNS Over HTTPS (DoH) on any system. This step-by-step guide ensures encrypted DNS traffic, preventing eavesdropping and manipulation, providing a more secure online experience. Follow these instructions to fortify your DNS queries and enjoy a safer browsing environment."
tags: ["DoH", "DNS"]
date: 2023-11-27T12:26:50+0800
thumbnail: https://www.itechtics.com/wp-content/uploads/2020/05/DNS-over-HTTPS-featured.jpg
lastmod: 2023-12-03T11:05:02+0800
---

## Introduction

Securing your DNS (Domain Name System) queries is an essential step in enhancing your online privacy. DNS Over HTTPS (DoH) encrypts your DNS traffic, preventing potential eavesdropping and manipulation. This guide walks you through configuring DoH on any system, such as linux, windows and android.

A simpler explanation is shown in the following diagram:

![DoH](./featured.webp)

## Browser DNS Over HTTPS?

Like Firefox based or chromium based browsers also have an option called "DNS Over HTTPS", but this will be set to global in this guide, which means that there is no need to set this option to browser, as all web dns are provided by system dns. Not just the browser dns.

### DNS Over HTTPS Provider

First of all, you need to find a DNS Over HTTPS (DoH) server provider, I recommend [Mullvad DoH](https://mullvad.net/en/help/dns-over-https-and-dns-over-tls). Otherwise this step is just different with the hostname and IP. You can use like IVPN, AdGuard, Google, Cloudflare, NextDNS and others.

### Mulvad DoH: Which one good for you?

It depends on your needs, in my case I would use base, honestly I want to watch porn. I've tested using all and I can't access most porn sites. But if you don't need it, use all.

Then if you only need the DoH functionality, just use dns hahahaha.
>dns.mullvad.net

| Hostname                 	| IPV4        	| Ads 	| Trackers 	| Malware 	| Adult 	| Gambling 	| Social media 	|
|--------------------------	|-------------	|-----	|----------	|---------	|-------	|----------	|--------------	|
| dns.mullvad.net          	| 194.242.2.2 	|     	|          	|         	|       	|          	|              	|
| adblock.dns.mullvad.net  	| 194.242.2.3 	| ✅   	| ✅        	|         	|       	|          	|              	|
| base.dns.mullvad.net     	| 194.242.2.4 	| ✅   	| ✅        	| ✅       	|       	|          	|              	|
| extended.dns.mullvad.net 	| 194.242.2.5 	| ✅   	| ✅        	| ✅       	|       	|          	| ✅            	|
| all.dns.mullvad.net      	| 194.242.2.9 	| ✅   	| ✅        	| ✅       	| ✅     	| ✅        	| ✅            	|

For the github repository see:

{{< github repo="mullvad/dns-blocklists" >}}

## Step-by-Step Guide on Linux (systemd)

{{< alert >}}
This steps only works on a systemd init based Linux system. such as arch linux.
{{< /alert >}}


1. **Enable systemd-resolved and start:**

   Open a Terminal and ensure that systemd-resolved is enabled and start:
   ```bash
   sudo systemctl enable systemd-resolved
   sudo systemctl start systemd-resolved
   ```

2. **Edit systemd-resolved Configuration:**

   Edit the systemd-resolved configuration file with your preferred text editor:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```

4. **Add DoH Servers:**

   In the opened file, add the following lines at the bottom under [Resolve]. Uncomment (remove #) the line corresponding to your preferred DNS server option:
   ```ini
   DNS=194.242.2.4 #base.dns.mullvad.net
   DNSSEC=yes
   DNSOverTLS=yes
   Domains=~.
   ```

   >Note: If you are currently using VPN of your system. `DNSOverTLS` should not be used as `yes`, set this to `opportunistic`. If it is set to  `yes`, you won't  able to use network.

   >Note: Enabling DNSSEC is optional, but it may cause issues with websites having incorrect DNSSEC information.

5. **Save and Exit:**

   Save the file by pressing `Ctrl + O` and then `Enter`, and then exit with `Ctrl + X`.

6. **Create Symbolic Link:**

   Create a symbolic link to the file using the following command in the Terminal:
   ```bash
   sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
   ```

7. **Restart systemd-resolved:**

   Restart systemd-resolved to apply the changes:
   ```bash
   sudo systemctl restart systemd-resolved
   ```

8. **Restart NetworkManager:**

   Restart NetworkManager for the changes to take effect:
   ```bash
   sudo systemctl restart NetworkManager
   ```

9. **Restart dhcpcd:**

    Restart dhcpcd for the changes to ensure take effect:
    ```shell
    sudo systemctl restart dhcpcd
    ```

9. **Verify DNS Settings:**

   Verify the DNS settings with:
   ```bash
   resolvectl status
   ```

   You should see `Current DNS Server:` output is your input IP.

11. **Test ping response:**

    If your setup is fine, try pinging any website and you should get a response.
    ```shell
    ping gentoo.org
    ```

12. **Test Mullvad DoH works well:**

    For Mullvad, refer to the [official Mullvad website](https://mullvad.net/en/check) to perform a check. Otherwise, use this command:
    ```
    resolvectl query gentoo.org
    ```

    the output should said:

    ```shell
    Data was acquired via local or encrypted transport: yes
    ```

## Step-by-step instructions on Windows

On Windows, you do not need to use the command line to complete the process. Windows already has a GUI for this process.

1. **Accessing Settings:** 
Open the Settings menu on your Windows system.
   ![Settings](./win11/Setting%20open.png)

2. **Navigating to Network & Internet Settings:** 
In the Settings menu, locate and click on `Network & Internet.`
     ![Network & Internet](./win11/Setting-network.png)

3. **Selecting Your Network:** 
Under `Network & Internet`, choose your preferred network, typically labeled as Ethernet for wired connections.
     ![Ethernet](./win11/Ethernet.png)

4. **Editing DNS Settings:** 
Click on the `Edit` option for IP settings, specifically focusing on the IPv4 DNS Server.
     ![Edit](./win11/Edit-all.png)

5. **Switching to Manual DNS Configuration:** 
Change the DNS configuration from `Automatic` to `Manual.`
     ![Manual](./win11/Manual.png)

6. **Setting Preferred DNS:** 
Update the "Preferred DNS" section with your chosen DNS address. For Windows, use one of the following Mullvad DoH addresses:
     - 194.242.2.2 - [dns.mullvad.net/dns-query](https://dns.mullvad.net/dns-query)
     - 194.242.2.3 - [adblock.dns.mullvad.net/dns-query](https://adblock.dns.mullvad.net/dns-query)
     - 194.242.2.4 - [base.dns.mullvad.net/dns-query](https://base.dns.mullvad.net/dns-query)
     - 194.242.2.5 - [extended.dns.mullvad.net/dns-query](https://extended.dns.mullvad.net/dns-query)
     - 194.242.2.9 - [all.dns.mullvad.net/dns-query](https://all.dns.mullvad.net/dns-query)

7. **Activating DNS Over HTTPS:** 
Enable the DNS over HTTPS option.

8. **Saving Changes and Adding Alternate DNS:** 
Save your changes. Optionally, you can add an `Alternate DNS` using a different address for redundancy.
     ![edit-all](./win11/example.png)

9. **Confirming Encryption:** 
After editing, ensure that the "IPv4 DNS Server" displays as `encrypted.`
     ![done](./win11/Done.png)

10. **Verification:** 
Check the DNS status on [Mullvad DNS](https://mullvad.net/en/check) to confirm that your DNS results are no leaked.

## Step-by-step guide for Android

Android is easier. Also has a GUI for it.

1. Start `Settings` and click on `Connections`:

![Settings](./android/Settings.png)

2. In `Connections` click on `More Connection Settings`.

![More Connection Settings](./android/More%20Connection%20Settings.png)

3. Click `Private DNS`, default is `Automatic`.

![Private DNS](./android/Private%20DNS.png)

4. Enter your preferred hostname. In this case I will use `base.dns.mullvad.net`.

![Enter hostname](./android/Enter%20host%20name.png)

## Conclusion

By following these steps, you've configured DNS Over HTTPS using systemd-resolved on your system, enhancing your privacy and securing your DNS queries. If you encounter issues, try the opportunistic DoH setting or experiment with different DNS server options.

## References

- [DNS over HTTPS and DNS over TLS](https://mullvad.net/en/help/dns-over-https-and-dns-over-tls)
- [What Is DNS-Over-HTTPS And How To Enable It On Your Device (Or Browser)](https://www.itechtics.com/dns-over-https/)
- [DNS over TLS with systemd-resolved](https://askubuntu.com/questions/1092498/dns-over-tls-with-systemd-resolved)
- [Guide on how to enable dot (dns over tls) on systemd-resolved.](https://www.reddit.com/r/linux/comments/us0h00/guide_on_how_to_enable_dot_dns_over_tls_on/)
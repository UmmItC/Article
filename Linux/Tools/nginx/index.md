---
author: "UmmIt"
title: "Setting Up NGINX on Linux VPS: Host a Website and Configure SSL with Self-Signed TLS (Cloudflare)"
description: "Learn how to set up NGINX on a Linux VPS, host a website, and secure it with SSL using a self-signed TLS certificate through Cloudflare. Follow our step-by-step guide to ensure your website is up and running securely."
tags: ["NGINX", "Web"]
date: 2023-09-11T04:00:50+0800
thumbnail: https://c93fea60bb98e121740fc38ff31162a8.s3.amazonaws.com/wp-content/uploads/2016/04/nginx.png
lastmod: 2024-04-29T23:53:00+0800
---

## Introduction

In the world of web servers, NGINX stands out as a powerful and efficient choice for hosting websites and web applications. Whether you're running a personal blog, a small business website, or a complex web application, NGINX is a versatile tool that can handle the job with ease. In this blog post, we'll walk you through the process of setting up NGINX on a Linux VPS, hosting a website, and securing it with SSL using a self-signed TLS certificate through Cloudflare.

## Step 1: Provision a Linux VPS

Before you can set up NGINX, you'll need a virtual private server (VPS) running a Linux distribution of your choice. Popular options include Ubuntu, CentOS, and Debian. You can choose a VPS provider like DigitalOcean, Linode, or AWS to provision your server.

## Step 2: Connect to Your VPS

Once your VPS is up and running, you'll need to connect to it using SSH. Open your terminal and use the following command to connect to your VPS:

```bash
ssh username@your_server_ip
```

Replace `username` with your server's username and `your_server_ip` with the actual IP address of your VPS.

## Step 3: Update Your System

It's essential to keep your server's software up to date for security and stability. Run the following commands to update your server:

```bash
sudo apt update
sudo apt upgrade
```

Replace `apt` with `yum` if you're using CentOS.

## Step 4: Install NGINX

Installing NGINX on Linux is straightforward. Use the package manager for your distribution to install NGINX. For Ubuntu, you can run:

```bash
sudo apt install nginx
```

For CentOS:

```bash
sudo yum install nginx
```

## Step 5: Start NGINX and Enable Auto-Start on Boot

After installation, start NGINX and enable it to start automatically on boot:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Allow Nginx Full in Firewall

Now, need to allow traffic on the Nginx Full port. Run the following command to allow incoming traffic on port 443:

```bash
sudo ufw allow 'Nginx Full'
```

and reload the ufw firewall:

```bash
sudo ufw reload
```

This step ensures that your NGINX server can receive traffic.

## Adding SSL/TLS Encryption with Cloudflare

Now that you've set up NGINX to serve your website, it's time to enhance security by encrypting the connection between your server and visitors' browsers using SSL/TLS. Cloudflare offers a convenient way to manage SSL/TLS certificates and provides additional security features like DDoS protection and web application firewall (WAF). Follow these steps to integrate Cloudflare with your NGINX server:

### Step 6: Sign Up for Cloudflare and Add Your Website

If you haven't already, sign up for a Cloudflare account and add your website to the dashboard. Cloudflare will guide you through the process of updating your domain's DNS settings to point to their servers.

### Step 7: Select SSL/TLS Encryption Mode

Navigate to the SSL/TLS section in your Cloudflare dashboard and select the "Overview" tab. Choose the encryption mode that best suits your needs. For maximum security, we recommend selecting "Full (strict)" mode, which ensures end-to-end encryption between Cloudflare and your origin server.

### Step 8: Generate an Origin Certificate

Cloudflare provides free SSL/TLS certificates, known as Origin Certificates, for securing the connection between Cloudflare and your origin server. Follow these steps to generate an Origin Certificate:

1. Log in to your Cloudflare dashboard.
2. Go to the SSL/TLS section and click on "Origin Certificates."
3. Select the appropriate options, such as the private key type (RSA 2048), hostnames, and certificate validity period.
4. Click "Create Certificate" to generate the certificate.

### Step 9: Install Origin Certificate on Your Server

Once you've generated the Origin Certificate, you need to install it on your NGINX server. SSH into your server and follow these steps:

1. Create the Origin Certificate file:

```bash
sudo vim /etc/ssl/cert.pem
```

2. Paste the Origin Certificate key into the file and save it.

3. Create the Private Key file:

```bash
sudo vim /etc/ssl/key.pem
```

4. Paste your Private Key into the file and save it.

### Step 10: Update NGINX Configuration

Update your NGINX configuration file to use the installed SSL/TLS certificates. Open/Create the configuration file:

```bash
sudo vim /etc/nginx/conf.d/yoursite_me.conf
```

Replace the SSL certificate and key paths with the paths to your Origin Certificate and Private Key files, and ensure the `server_name` matches your directory structure:

```conf
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;

  ssl_certificate       /etc/ssl/cert.pem;
  ssl_certificate_key   /etc/ssl/key.pem;

  location / {
          try_files $uri $uri/ =404;
  }

  server_name yoursite_me yoursize.me;

  root /var/www/yoursite_me/html;
  index index.html;
}
```

### Step 11: Test and Restart NGINX

Before proceeding, test your NGINX configuration to ensure there are no syntax errors:

```bash
sudo nginx -t
```

If the test is successful, restart NGINX to apply the changes:

```bash
sudo systemctl restart nginx
```

### Step 12: Verify Cloudflare Settings

Finally, return to your Cloudflare dashboard and verify that SSL/TLS encryption mode is set to "Full (strict)" to enforce secure communication between Cloudflare and your NGINX server.

With these steps completed, your website is now securely encrypted with SSL/TLS, providing enhanced security and privacy for your visitors.


## References

- [How To Host a Website Using Cloudflare and Nginx on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-host-a-website-using-cloudflare-and-nginx-on-ubuntu-22-04)
- [How To Install Nginx on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04#step-5-setting-up-server-blocks-recommended)

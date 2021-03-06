---
title: How to setup Wireguard VPN from a private VPS.
date: 2020-09-27
layout: post
permalink: /vpn-from-vps/
tags: tech
---

Bottom Line — using a VPN is great for online privacy. Hosting it on a VPS ensures your IP won't be blacklisted. It's easy to setup with an automated script.

I previously wrote about how VPNs can protect you from from packet sniffing on public WIFIs and from ISPs knowing which sites you visit (some sell this information).

You can quickly set up a VPN server on a VPS using an automated script.

# Buy a VPS
Vultr's cheapest VPS droplet is $3.50. Digital Ocean's is $5. You can get a cheap one from [VirMach at $1.25 / month](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjKiYWa5IrsAhXlY98KHXEKBTIQFjAAegQIBBAB&url=https%3A%2F%2Fbilling.virmach.com%2Fcart.php&usg=AOvVaw3U7Q7qDAKWMsQSU1reS0Yu).

# Wireguard Script
There's an [open source script (angristan/wireguard-install) on GitHub](https://github.com/angristan/wireguard-install) that will automatically install Wireguard on your server.

At the time of this writing --- the script supports Debian 10 and that's what I prefer to use.

Once you make a purchase, it's time SSH into your VPS.

`ssh root@xx.xx.xx.xx`

Paste in your root password. You'll find this in your VPS control panel.

Make sure you install `curl`.

`apt-get install curl`

Run the script from the github page:

`curl -O https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh`

`chmod +x wireguard-install.sh`

`./wireguard-install.sh`

Unless you know what you're doing, you can leave the requested fields as blanks. They will use default values.

# Creating Clients
The script will ask you to provide a client name. You can call it whatever you want.

Each client will have their own configuration file. For example `client01.conf`.

Let's say you have two laptops and a phone that you'd like to configure this VPN for. You'll need a total of 3 configuration files.

To generate additional configuration files, run the script again and fill out the details.

`./wireguard-install.sh`

# Transferring configuration files from the VPS

Check the root directory on your vps:

`ls`

You'll see your configuration files.

We'll use scp (secure copy files) to transfer them from the vps server to your local machine for setup.

Let's say you generated a client configuration file called `client1.conf`.

Open up another LOCAL terminal window and type in the following:

`scp root@4xx.xx.xx.xxx:client1.conf /Users/admin/desktop`

It will connect to your VPS and ask it to transfer `client1.conf` to your local `/Users/admin/desktop` folder.

It will ask your for the root password. Simply paste it in. The transfer should be successful!

Transfer all the other configuration files you created to your local machine one by one.

# Download WireGuard
Download [WireGuard](https://www.wireguard.com/) and import the configuration file.
A handy tip is to send yourself an email with all the other configuration files. You can easily access your email on your other devices and load up those configuration files.

# Test VPN

Ensure that your VPN IP address [is NOT blacklisted](https://whatismyipaddress.com/blacklist-check).

Run [ipleak.net](https://ipleak.net/) to make sure your DNS isn't leaking information. You'll know because your location won't be present anywhere on that page.

Finally, do a [speed test to check your VPN speed](https://www.speedtest.net/). It will be slower than your actual internet speed but it should be fast enough. If not, you can choose to look for another VPS provider.
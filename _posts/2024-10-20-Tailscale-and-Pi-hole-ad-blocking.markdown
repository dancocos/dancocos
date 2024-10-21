---
layout: post
title:  "Tailscale DNS Scripts"
date:   2024-10-20 00:00:00 -0500
categories: [TIL, macOS, sysadmin, security, DNS]
---

I'm a big fan of [Tailscale](https://tailscale.com) and when I'm not at home I still like to use the [Pi-holes](https://pi-hole.net) I have setup at home to block ads and malicious sites. 

For the sake of redundancy, I'm a sys admin by trade, I have two Pi-holes running but this will work just as well with one. 

Following the [official documentation](https://pi-hole.net) once you have your pi-hole configured the next step is to configure Tailscale. If they aren't already [setup your nodes as Machines](https://tailscale.com/kb/1031/install-linux) in your network. You may also want to disable key expiry, this comes with some minimal security risks, but will prevent you from having to login every 90(?) days to re-authenticate. 

From the [Tailscale Machines page](https://login.tailscale.com/admin/machines) copy and note the IPv4 and IPv6 addresses.
![Machines in Tailscale](/images/ts-machines.png)

Then head on over to the [Tailscale DNS page](https://login.tailscale.com/admin/dns)

![DNS in Tailscale](/images/ts-dns.png)

On this page Add nameserver --> Custom add all of the IPs from your the Machines page. 
Make sure to click "Override local DNS."

On all of your devices where you'd like to minimize ads and tracking enable Use Tailscale DNS settings
![Client Settings in Tailscale](/images/ts-settings.png)

You can run Tailscale on all of your devices now with the benefit of ad blocking. This is especially nice if you use it on your phone because usually when you are on the cell network you cannot override your providers DNS but since Tailscale is treated as VPN it will use your Pi-hole DNS. 

If you ever run into a situation where you're not sure if blocking is causing problems just disconnect from Tailscale.

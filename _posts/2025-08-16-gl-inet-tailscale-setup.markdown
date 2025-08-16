---
layout: post
title:  "GL.iNet Tailscale Config"
date:   2025-08-16 00:00:00 -0500
categories: [tailscale, home network, wifi, router, security, configuration]
---

I recently picked up the [Flint 3](https://www.gl-inet.com/products/gl-be9300/) router, running [OpenWrt](http://openwrt.org/), to replace my aging home networking gear.

The version of Tailscale supported by GL Inet/Openwrt isn't very current. Thanks to [Admonistrator](https://github.com/Admonstrator/) [Tailscale Update Script for GL.iNet Routers](https://github.com/Admonstrator/glinet-tailscale-updater) exists

## BEFORE YOU RUN THE SCRIPT DO THE FOLLOWING

Go to Applications --> Click to Enable Tailscale
Bind your account, it will process for a few minutes and should give you a link. Click on the link THEN run the script above.

![Tailscale config menu](/images/tailscale-config.png)

Now you can use a more up to date version of Tailscale, keep the scripts around because you'll want to run them peridocially to keep up to date.
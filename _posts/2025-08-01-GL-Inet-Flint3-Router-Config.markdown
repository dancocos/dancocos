---
layout: post
title:  "Basic Flint 3 Router Security"
date:   2025-08-01 00:00:00 -0500
categories: [home network, wifi, router, security, configuration]
---

I recently picked up the [Flint 3](https://www.gl-inet.com/products/gl-be9300/) router, running [OpenWrt](http://openwrt.org/), to replace my aging home networking gear. I didn't find a complete guide to basic configuration I'd deem manditory out of the box.

The GL Inet "basic" interface is on ports 80/443 and the LuCI UI on 8080/8443. They seem to configue the same things just with different interfaces and different levels of details. On to enabling ssh with keys.

Using the LuCI interface you can add keys via the System --> Administraion menu. On the SSH Access tab, I suggest turning OFF

* Password authentication
* Allow root logins with password
* Allow the root user to log in with password

On the SSH Keys tab you can, unsuprisingly add ssh keys.

On the HTTP(S) Access tab I suggest you enable "Redirect to HTTPS"

Save and apply everything, you should now be able to ssh in with root@hostname

Next up Tailscale and SSL Certs.

Follow the instructions in my post [GL.iNet Tailscale Config](/2025/08/16/gl-inet-tailscale-setup.html) before doing anyhing else with Tailscale.


For the basic GL Inet web interface and the Luci interface I run the following commands, of course change gl-be9300.EXAMPLE.ts.net to your host name.

```
/usr/sbin/tailscale cert gl-be9300.EXAMPLE.ts.net
cp gl-be9300.EXAMPLE.ts.net.crt /etc/nginx/nginx.cer
cp gl-be9300.EXAMPLE.ts.net.key /etc/nginx/nginx.key
service nginx restart
cp gl-be9300.EXAMPLE.ts.net.crt /etc/uhttpd.crt
cp gl-be9300.EXAMPLE.ts.net.key /etc/uhttpd.key
service uhttpd restart
```

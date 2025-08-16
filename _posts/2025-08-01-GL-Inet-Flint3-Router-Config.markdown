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

Follow the instructions in my post [GL.iNet Tailscale Config](2025/08/16/gl-inet-tailscale-setup.html) before doing anyhing else with Tailscale.


For the basic GL Inet web interface

```
/usr/sbin/tailscale cert gl-be9300.EXAMPLE.ts.net
cp gl-be9300.EXAMPLE.ts.net* /etc/nginx/
ls -al /etc/nginx/
vim /etc/nginx/conf.d/gl.conf
```

Change the following lines in the file to point your cert/key files (around line 21/22)
```
    ssl_certificate /etc/nginx/gl-be9300.EXAMPLE.ts.net.crt;
    ssl_certificate_key /etc/nginx/gl-be9300.EXAMPLE.ts.net.key;
```

Now check your nginx config with `nginx -t` you should see the following
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

If test doesn't show any problems then restart nginx `service nginx restart`

For the Advanced/LuCI interface

First backup the current key and cert just incase you screw something up.
```
cp /etc/uhttpd.crt ~/
cp /etc/uhttpd.key ~/
```

Then copy the certs over and restart LuCI.
```
  cp gl-be9300.EXAMPLE.ts.net.crt /etc/uhttpd.crt
  cp gl-be9300.EXAMPLE.ts.net.key /etc/uhttpd.key
  service uhttpd restart
```

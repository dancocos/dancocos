---
layout: post
title:  "GL.iNet Repeater Expander Nonsense"
date:   2025-08-16 2:00:00 -0500
categories: [tailscale, home network, wifi, router, security, configuration]
---
# I added an [update](#update) at the end

I recently picked up the [Flint 3](https://www.gl-inet.com/products/gl-be9300/) router, and a [Slate 7](https://www.gl-inet.com/products/gl-be3600/) to replace my aging Asus networking gear.


One thing I find incredibly frustrating is the lack of clear documentation of how to configure the Flint 3 as the primary and the Slate 7 as the secondary.

Intially set it up as a WDS node but that seemed to break pretty often and it appeared that you lost access to the admin interface on the Slate 7.

Based on a comment from GL.iNet employee I went with repeater mode. The keys to getting this off the ground.

On the Slate 7 change all of the network names to match the SSID of your Flint 3 and connect to repeater mode.

Things that broke the configuration and required me to have to connect with an ethernet cable or reset the Slate 7.

* Do NOT enable MLO Wi-Fi on the Slate 7 this seems to break the repeater mode
* Do NOT enable IPv6 on the Slate 7 this also seems to break things

Other things to note I set the IP of the Flint 3 router to 192.168.50.1. I left the default 192.168.8.1 config on the Slate 3 this is the address it answers on even when in repeater mode, it is given a 192.168.50.x address from the Flint 3 but it won't answer on it. To make all of this a bit easier I have Tailscale installed on both routers and end up just using the hostnames to save me a lot of headache.

It does make me appreciate the simplicity of setting up the Asus routers in "mesh mode" where you simply made a few click in the app or web interface to add new nodes.

I'm still not sure if this is the best config but it seems to work for now. There are a few videos provided by GL.iNet but seem to be lacking in a few critical details like setting the SSIDs to all be the same.

# <a name="update"></a>update:
I went back to WDS mode mostly because I want everything on same subnet and I want the Flint 3 to handle all of heavy lifting, such as DNS and DHCP. I followed a suggestion on an openwrt forum and have a crontab that bounces the wifi if it goes down.
```
*/10 * * * * /bin/ping -c 1 192.168.50.1 || (/sbin/wifi down && /sbin/wifi up)

```
The only downside I've seen so far is that I can't connect via ssh or https over Tailscale despite the fact that it is running on the Slate and I can see in the console. 🤷‍♂️ If anyone knows how to make that work it would be much appreciated. Update I can get there via local IP.    

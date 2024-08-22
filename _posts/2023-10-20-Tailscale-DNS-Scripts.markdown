---
layout: post
title:  "Tailscale DNS Scripts"
date:   2023-10-20 00:00:00 -0500
categories: [TIL, macOS, sysadmin, security, DNS]
---

I'm a big fan of [Tailscale](https://tailscale.com) and when I'm not at home I still like to use the [Pi-hole](https://pi-hole.net) I have setup at home to block ads and malicious sites. The way I manage this is to set my DNS server to the TS IP 100.69.187.53 for my Pi-Hole.

This works well unless I'm out and about and on public wifi, especially if there is a captive portal, sometimes ["Things Fall Apart"*](https://en.wikipedia.org/wiki/Things_Fall_Apart) as the auth may need to use local DNS. You can solve this by --> Settings --> Wi-fi --> Details --> DNS (change the DNS) auth the portal and then change everything back. 

![Too Damn High Meme Guy with text "the number of clicks is too damn high!](/images/number-of-clicks-too-damn-high.png)

Of course there is a way to script this so I created the following two scripts in  `~/bin` the first line is for Wi-Fi the second is for LAN your interface names may vary.

in `dns-gen`
```
#!/bin/bash
networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
networksetup -setdnsservers "USB 10/100/1000 LAN" 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
```
in `dns-ts`
```
#!/bin/bash
networksetup -setdnsservers Wi-Fi 100.69.187.53 fd7a:115c:a1e0:ab12:4843:cd96:6245:b720
networksetup -setdnsservers "USB 10/100/1000 LAN" 100.69.187.53 fd7a:115c:a1e0:ab12:4843:cd96:6245:b720
```
Don't forget to make sure the scripts are in your path and to mark them `chmod 744`


*I found out about the book from title of the amazing album by The Legendary Roots Crew of the same name here's a [review of "Things Fall Apart"](https://pitchfork.com/reviews/albums/22132-things-fall-apart/)


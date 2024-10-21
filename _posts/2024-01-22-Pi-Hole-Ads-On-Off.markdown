---
layout: post
title:  "Let Me See Some Ads Real Quick"
date:   2024-01-22 00:00:00 -0500
categories: [macOS, sysadmin, DNS, pi-hole, ads, hacks]
---
## Note it doesn't have be this complicated [read here instead](/2024/10/20/Tailscale-and-Pi-hole-ad-blocking.html) wherein you can just disable your TS VPN temporarily

When at home and out and about my laptop makes its DNS queries via a [Pi-Hole](https://pi-hole.net) at my house, routed via [Tailscale](https://tailscale.com) this means I rarely see any ads and my ad tracking profile is reduced. 

Sometimes you may want to turn on ads. The most common use cases for me are trying to look at Google Analyics or ads, following some tracked links in emails and troubleshooting when certain sites don't work. You can disable this by logging into the Pi-Hole UI and clicking Disable Blocking link but this often involves several clicks and possibly authenticating. 

![Screenshot of the pi-hole disable blocking widget](/images/pi-hole-disable-blocking.png)


After some searching it turns out you can hit an API endpoint to enable/disable ad blocking and as for authentication the `/etc/pihole/setupVars.conf` file contains a password you can pass. (Yes this comes with risks but I'm on my own private network and the call is made over https and even if it was compromised I'm not too worried about the threat coming from inside my house.)

## Note it doesn't have be this complicated [read here instead](/2024/10/20/Tailscale-and-Pi-hole-ad-blocking.html) wherein you can just disable your TS VPN temporarily

As such I created two scripts to enable/disable ads

```
~:cat bin/p-down.sh
#!/bin/bash
curl -X GET 'https://pi.hole/admin/api.php?disable=90&auth=[fromConfig]'
```

```
~:cat bin/p-up.sh
#!/bin/bash
curl -X GET 'https://pi.hole/admin/api.php?enable&auth=[fromConfig]'
```

Don't forget to make sure the scripts are in your path and to mark them `chmod 744`

Replace `pi.hole` with the hostname of your Pi-Hole, the number in the query string after disable is number of seconds to disable ads and the [fromConfig] is the hash(?) you can find in `/etc/pihole/setupVars.conf` which you can find with 
 
 `grep WEBPASSWORD /etc/pihole/setupVars.conf`
 
 or if you're really lazy 
 
 `grep WEBPASSWORD /etc/pihole/setupVars.conf | cut -d '=' -f 2`

To increase my laziness even more I added buttons to my [Stream Deck](https://www.elgato.com/us/en/p/stream-deck-mk2-black), note to get scripts to run they may have to end in `.sh`

![Image of Stream Deck with buttons for ads on and ads off](/images/stream-deck-ads.png)
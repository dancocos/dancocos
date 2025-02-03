---
layout: post
title:  "Jellyfin SSL Tailscale"
date:   2025-02-02 00:00:00 -0500
categories: [home, jellyfin, scripts]
---

I recently setup [Jellyfin](https://jellyfin.org) on an Ubuntu box at home. I wanted to use SSL certs and [Tailscale](https://tailscale.com) for the web interface.

You'll need to create a cert in pfx/pkcs12 format

Replace the servername with yours I put mine in a script
```
#!/bin/bash
cd /root
/usr/bin/tailscale cert server.example-example.ts.net
openssl pkcs12 -export -inkey /root/server.example-example.ts.net.key  -in /root/server.example-example.ts.net.crt  -out /etc/jellyfin/server.example-example.ts.net.pfx  -passout pass:
chmod 755 /etc/jellyfin/server.example-example.ts.net.pfx
service jellyfin restart
```


Then enable https
![Enable https](/images/jf-https.png)


Once you've verirfied everything works you can force https
![Require https](/images/jf-https-example.png)



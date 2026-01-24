---
layout: post
title:  "Tailscale Service Jellyfin Proxy"
date:   2026-01-24 2:00:00 -0500
categories: [security, configuration, dns, privacy, tailscale]
---

If it isn't clear already I love [Tailscale](https://tailscale.com/) I setup a [Jellyfin](https://jellyfin.org) server on my home network, [read about setting up ssl](/2025/02/02/jellyfin-ssl-tailscale.html), the server running Jellyfin has a few other services running as well so Jellyfin is on port 8920 it's annoying to have add a port to a URL and when serving up "family videos" to friends and family it's a step beyond what they would think is reasonable. In comes Tailscale services. 

First create a [tag](https://login.tailscale.com/admin/acls/visual/tags) for this service. I named mine "jellyfin" you can assign a Tag Owner and description if you'd like. 

Next [define a service](https://login.tailscale.com/admin/services/new). I also named this "jellyfin" this name is what how you'll access it in your browser https://jellyfin.name-example.ts.net/ 

Note the port should be 443 this is the port the TS service uses **NOT** the port you're serving on your instance.
![define a service](/images/jellyfin-service.jpeg)

Add the tag jellyfin as well. 

Also add the tag jellyfin to the machine.

Now ssh into the instance running jellyfin `sudo -i` and run the following 
`tailscale serve --service=svc:jellyfin -https=443 https://myserver.example-name.ts.net:8920`

You should see something like this:
```
root@myserver:~# tailscale serve --service=svc:jellyfin -https=443 https://myserver.example-name.ts.net:8920
Available within your tailnet:

https://jellyfin.example-name.ts.net/
|-- proxy https://myserver.example-name.ts.net:8920

Serve started and running in the background.
To disable the proxy, run: tailscale serve --service=svc:jellyfin --https=443 off
To remove config for the service, run: tailscale serve clear svc:jellyfin
```

Open a browser and go to https://jellyfin.example-name.ts.net/ all done. 

<div class="tenor-gif-embed" data-postid="9978646096663271619" data-share-method="host" data-aspect-ratio="1" data-width="50%"><a href="https://tenor.com/view/shimmy-annie-murphy-alexis-alexis-rose-schitts-creek-gif-9978646096663271619">Shimmy Annie Murphy GIF</a>from <a href="https://tenor.com/search/shimmy-gifs">Shimmy GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>

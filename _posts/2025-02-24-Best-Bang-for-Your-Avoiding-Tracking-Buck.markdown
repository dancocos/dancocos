---
layout: post
title:  "Avoid Tracking - Best Bang for Your Buck"
date:   2025-02-02 00:00:00 -0500
categories: [advice, laity, explainer, privacy, security]
---

With all of the changes, aka the 💩 show going on in the US right now, a few people have reached out asking how to be more secure online and avoid tracking. They usually start off with "What VPN should I use?"

The money spent by VPN companies on marketing appears to have worked, people think that using a VPN will protect their privacy online and that obscuring their IP address will somehow make them secure. They also seem to imply that doing so will block cookies as evidenced by an ad I saw on the Metro.

![Cookies online are not blocked by a VPN](/images/cookies-ad.png)

This is not to say that the VPNs don't serve a purpose and that they aren't good, hell I pay for a Mullvad subscription. These are broad generalizations but close enough for the general public to understand what VPNs do. 

## Work
Workplaces will often have your computer connect via a VPN. This covers four things.

 * Puts all of your traffic through a secure tunnel to a server controlled by your job. 
 
 * Helps to insure that if you use an insecure protocol someone at the cafe can't snoop on your traffic. 
 
 * Usually has the benefit of forcing you to use their DNS which will let you find internal servers. 
 
 * Lastly it allows them to control and see where you go online. This isn't really bad per se it's their computer they control it. 

 ## Home
 Using a VPN at home (or from a cafe) does the following. This makes a few assumptions again but is generally true.

 * Funnels all of your traffic via a tunnel to the VPN sever, this hides where you are going from the cafe.

 * Usually you can chose the VPN exit location. This means you can use an IP address in another location. A common example is saying you're in the US to watch localized Hulu.

 * Make you appear to the sites you are visiting that you are coming from a different location. 



> The tl;dr for VPNs it that you can appear to be calling from another place and share resources with other devices on your VPN but it doesn't really block tracking.

## How I Block Ads at the DNS Level

The VPN doesn't really stop companies from tracking you, you're basically spoofing the caller id but when someone picks up the line on the other side you're still saying "Hi, I'm Dan how are you doing?" If you want to stop this type of tracking, usually done through cookies, you can mitigate a LOT of this via ad blockers. There are several browser plugins that can block ads and those work pretty well, but I prefer to nip to the problem in the bud by blocking ads at the DNS level. 

When you load a web page it make a series of calls to multiple servers. I'm using CNN as an example but this is how most modern web pages work so there isn't anything particularly nefarious about what CNN is doing. 

 
![CNN Loading page](/images/cnn-loading.png)


I used [HTTP Toolkit](https://httptoolkit.com/) to gather all of the requests if you look you can see the initial request, GET, for www.cnn.com with the [status 200](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#2xx_success) which means "OK" Let's skip down and look at one of the GETs with a 🚫 icon. My snooping tool tells me me this about my request for `https://get.s-onetag.com/c15ddde9-ec7d-4a49-b8ca-7a21bc4b943b/tag.min.js` 

>The upstream server hostname could be not found, so HTTP Toolkit did not forward the request.
>This typically means the host doesn't exist, although it could be an issue with your DNS or network configuration.

This means that when it tried to look up the server using DNS (what translates names to IPs) that it came back empty. I wouldn't call this an "issue" with my DNS or network configuration but instead a feature. I use the software tool [Pi-hole](https://pi-hole.net) to lookup my DNS requests. Pi-hole maintains a list of servers that are specifically used for ad-tracking and when requested tells my browser "nope that doens't exist" This is very effective in cutting down tracking because requests to share your info are not able to phone home. 

How pervasive is this you may ask? Of the 60 requests my browser made to request the page 40 of them were to tracking services.  (╯°□°）╯︵ ┻━┻) 	

Why do I prefer DNS blocking vs browser ad blocking you may ask? Well because it not only works in my browser, but in my email, on the devices I use around the house including things you never really think about, like your TV. 

What are the downsides blocking ads on the DNS level? It will sometimes break things, you can make exceptions for those sites you want to report back to or in the case of Pi-hole temporarily disable the blocker for a while to see if it is even problem.  Some sites can detect that you're blocking ads and will show you a popup. Sometimes it will let you through trying to guilt you into unblocking their site and other times it won't. ¯\_(ツ)_/¯ 

![Imgur add blocking modal](/images/imgur-admiral.png)


I realize not everyone has the desire or skills to run a Pi-hole what can they do? There are services that offer similar functionality to a Pi-hole. One of them that a lot of people recommend is [NextDNS](https://nextdns.io) I've not personally used them nor am I getting paid or a referral bonus to recommend them but they have a free tier you can try and the paid tier is $19.90 a year. 

Does this mean you can't be tracked online all together? Not really the EFF's [Cover Your Tracks](https://coveryourtracks.eff.org) will give you a better idea of what's going on.

Have questions or is something I said blatantly wrong shoot me an email.   
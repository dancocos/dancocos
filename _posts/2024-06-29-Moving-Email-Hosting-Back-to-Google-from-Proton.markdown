---
layout: post
title:  "Moving Hosting to Google Workspace from Proton"
date:   2024-06-29 00:00:00 -0500
categories: [hosting, email, security, sysadmin]
---

I gave Proton Mail the ole college try for my mail hosting but ended up moving Google Workspace. 

Here is a history of my email that no one cares about, but you're going to read about anyway. (I'm going to make this like on of those recipe posts that has a bunch of irrelevant information and you have to scroll ot the bottom to get the real info.) 

For a brief time in the late 90s I lived in Northern Virginia despiting living very close to the HQs of AOL and PSINet and cable and DSL being available high speed options at the time much to my disappointment neither were available to my house. So I was stuck with dial-up. Wifi was still pretty new at the time and I knew of at least one neighbor who got a T-1 wired to his house setup an AP and ran a wireless ISP. While appealing it seemed like a heavy investment of both time and money. After a lot of research I found an acceptable solution. 

The ISP [Speakeasy](https://en.wikipedia.org/wiki/Speakeasy_(ISP)) would run a dry copper pair to my house and give me an always on 144/144kbit/s IDSL line for [$89.95](https://en.wikipedia.org/wiki/ISDN_digital_subscriber_line) a month along with a static IP *AND* you were allowed to run your own servers! As long it wasn't porn/warez or anything illegal they were cool with it. So I of course ran with it. I had two boxes up and running, primary and secondary MX servers with one of them also hosting my personal website. This was nerd heaven in control of everything, this was before DKIM and SPF records were a thing and hosting your own mail was reasonable. I used `elm` as my mail client and even had [Squirrel Mail](https://www.squirrelmail.org) running for remote access with a self-signed cert for https!

Around this time two big things happened. I moved back to DC proper and Gmail became a thing. Not really wanting to manage my mail servers during the move, especially since they would likely be offline for a few days until the internet was hooked up the new place I ended up moving everything to Textdrive which no longer exists :-(. At some point Google offered a service where you could use your own domain with Gmail for free. So I setup dcdan.com and dancocos.com with Google and called it a day. I used IMAP to get my email so all was good and kept it this way for while. 

Then at some point Google changed their policies and were going to start charging, so I setup a forwarding service via [No-IP](https://www.noip.com) to my regular Gmail account and called it a day. The main downside of this was the if I tried to send an email from dan@dcdan.com it would come from Gmail "on behalf of" which is pretty janky.

So after reading a bit about it I decided to try out Proton Mail and it was fine, they are a solid hosting provider but as my two year subscription is about to expire I've decided the cons are too many to continue. 
Proton Mail (at least for the plan I was on)

Pros:
* They allow hosting for up to three of your own domains
* They seem to have a pretty serious stance on security
* They've added a lot of nice features and extra services (VPN, Password Manager, Secure Storage)

Cons: 
* To use a regular email client you have run Proton Bridge on your computer, the first couple of versions of this were awful but now it's pretty solid
* Trying to use the calendar with a native app is basically read only
* They aren't alway as forthright as they could be with their [policies](https://arstechnica.com/information-technology/2021/09/privacy-focused-protonmail-provided-a-users-ip-address-to-authorities/)

So I've decided to move to Google Workspace and here's where the "recipe" of this post comes into play. 

If you have multiple domains and you use them mostly for email this is what worked for me. Setup one as the primary domain dancocos.com then for the other domain setup a "user alias domain," dcdan.com. The long and short of it is that if you're just using the second domain for an email address, as I'm doing, this should work out well for you and has the benefit of only needing to pay for one account. If you have more complex needs and the info between the domains needs isolation you'll probably want to go with "Secondary domain."

Google does a pretty good job of hand holding you through the TXT and MX records part of the transfer, one thing they didn't cover though was adding in DMARC records, which were easy enough to setup on my own.

Here are takeaways from the email setup. You can setup the email aliases as send from in Gmail, this works when using the Gmail browser app, and Mail.app on macOS, this is not supported using Mail.app on iOS if you want to send from an alias you'll have to use the Gmail app. It's also worth noting that even when I send from dcdan.com, dancocos.com still shows up in the full headers. So if you're concerned about info leakage be aware. 

Next up getting your old emails into your account. There are ton of migration tools and blog posts. I initially went down the path of using Thunderbird as this what everyone said seemed to work but I kept on getting timeouts. I then remembered that Mail.app can import mbox format. 

File-->Import mbox in Mail.app is what I used it will pull it into a new folder "On My Mac" with then name of the mbox. I used this for the two Google Takeout exports I had from the first time I hosted with them.

Now for the more complex part. Proton provides a cli based export tool which works pretty well. Depending on the size if your inbox it can take a while, for me a couple of hours, but everything ran as expected. It dumps everything into a fold wherein every email it pulls is an eml file along with a metadata.json file in the format like this 

```
zzzrvsLoxD5Fz07cJX7gmNo69i7NRQW2tZsyMh21htBiXoYNhDZ-rSyTTbq7II5zF9tPOuiUgFFyo6qsrjUeqw==.eml
zzzrvsLoxD5Fz07cJX7gmNo69i7NRQW2tZsyMh21htBiXoYNhDZ-rSyTTbq7II5zF9tPOuiUgFFyo6qsrjUeqw==.metadata.json
```

This means there will be two files for ever email, in my case 

```
ls -la | wc -l
  375104
```

It's rare that one has to wait a few minutes for `ls` to show a result. Now you _could_ copy each of these one by one into your inbox but that would take forever. Luckily I found that [Nicola Mastrandrea](https://github.com/nick88msn) wrote [eml2mbox](https://github.com/nick88msn/eml2mbox) which is amazingly fast, so much so that I did a double take to make sure it worked. It converted 187,551 eml files into a 12gb mbox folder in 3 minutes and 17 seconds. I then imported that into Mail.app as described above. 

The final step was to create labels in Gmail and then drag and drop the emails into Gmail via the Mail.app UI, this is part that takes the longest you can monitor the process with the Activity Window on Mac with `⌥ ⌘ 0`

One last thing to note, is that until your first payment hits if you're logged in your newly created Google Workspace account things like YouTube and Google Voice will say they aren't setup or show you errors until you're out of the trial phase.

 *Update*: You have to spend at least $30, I spent $30.01, paying early so I now have a credit for my first few months. After the pre-payment was made YouTube started working within 24 hours. To pre-pay Admin Console --> Billing --> Payment accounts --> Click on your account ID number --> Pay early (Put in $30.01)

If I've misstated or missed anything please let me know. 
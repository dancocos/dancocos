---
layout: post
title:  "Hosting Your Site on Github with Jekyll"
date:   2024-06-29 00:00:00 -0500
categories: [github, hosting, jekyll,TIL]
---

This may be obvious to a lot a of people but I'm new to Jekyll, Ruby and hosting on Github. Getting the basic site up and running was pretty easy and while I wanted to make some tweaks here and there I could live without them. 

Recently though I read about [Counterscale](https://counterscale.dev) and I wanted to add it to my site, it should have been simple to just add the `<script>` tags to the header but everything I read online talked about making a change to the files in `_layouts` 


I could not for the life of me find this folder or the files. 

After a ton of searching this [Stack Overflow]([https://stackoverflow.com/questions/51253614/home-layout-missing-in-layouts-folder-but-works) answer finally make it clear `bundle show minima` let me know where the templates files existed on my laptop. I then copied over the `_includes` `_layouts` and `_sass` files to the root of my repo and now I could make changes to the templates. I added the script.

Note to do it the "right way" wrap the Counterscale script in a `if jekyll.environment == 'production'` block so that you only collect stats when deployed.

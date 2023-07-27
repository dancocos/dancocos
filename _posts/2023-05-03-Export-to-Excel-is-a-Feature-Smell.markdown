---
layout: post
title:  "Export to Excel is a Feature Smell"
date:   2023-05-03 00:00:00 -0500
categories: [hot-take, Excel, software dev]

---

One feature that is perennially requested is "We need to export to Excel" this is a feature smell, similar to a [code smell](https://en.wikipedia.org/wiki/Code_smell). 

*Ask yourself?*


  * Why are your users exporting to Excel? 
  
  * What are they doing in Excel that they can't do in your app? 
  
  * If there are reports or charts that aren't available in your app add them. 
  
  Exporting to Excel is a horrible idea because once the data is out of your system it's immediately  out of date and stands a real possibility of being [wrong](https://www.wsj.com/articles/stop-using-excel-finance-chiefs-tell-staffs-1511346601).  

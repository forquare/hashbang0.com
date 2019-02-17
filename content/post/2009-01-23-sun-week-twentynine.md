---
date: "2009-01-23T18:27:22"
title: "Sun - Week Twenty-nine"
tags: ["b445","BIOS","dumb terminal","SCSI/SAS","SVM","w1100z","x2200"]
categories: ["Sun"]
---

This week was very VERY busy!

Monday starts with me in the lab net booting a w1100z, this old box doesn't output it's BIOS over the serial cable, so a keyboard and screen was attached to allow me to net boot the thing.
I spent the end of the morning and all afternoon finishing off the cable store. All the boxes of SCSI/SAS cables are all in their own box, and are all labelled! Only took three sessions in there!

Tuesday was spent looking into some outstanding tickets.

Wednesday I spent nearly all day working on two new 2200's. Neither of them wanted to give me any output on the serial port. I tried hooking up a monitor and keyboard and found that I could see the BIOS and where the OS should boot, but could not get to the SP. I reeled in an old dumb terminal (circa 1993), and found nothing on there.
After talking to a number of engineers and trying a few things, I reset the BIOS back to it's default settings, and then everything worked! Brilliant, a whole day wasted on these two bloomin' machine!

Thursday I got a ticket from a guy who I had been helping over the last few days. He wanted a machine with at least two hard disks and then wanted to mirror them with SVM on boot. If your a regular reader of the blog then you're already thinking "Well Ben, what this guy needs is your blog from [week fourteen][1]!" Indeed, I knew I had the info to do this, so a quick look at my blog I realised I had written a document about it for our internal documents system. I refreshed myself on what needed doing and then passed the box back to the engineer.
I was also asked to check some fibre connections in the lab, I replaced some cables and everything seemed OK.

Friday came with me not sitting in my seat for very long at all. I retired an old stable box called 'pint', the owner no longer wanted it, so after pulling the entry from NIS+ and our booking system, I retired the box to the recycling pile.
Two of the machines in the lab had been labelled incorrectly, or rather, they both had the same labels. I spent about half an hour tracing cables and looking at location lights to figure which one thought it was host A, and which one thought it was host D. I reprinted some labels and stuck the on the offending machine :wink:
I had another ticked regarding some fibre connections, after replacing the cabling the engineer found the problem had gone.
I had a ticket from [Lewis][2] asking to attach some storage to a couple of v445's this caused several problems due to the state of the machines when Lewis had picked them up, but these were rectified by several reboots and unplugging cables...

All in all, a busy week!

  [1]: /2008/10/10/sun-week-fourteen/
  [2]: http://www.lewiz.org/

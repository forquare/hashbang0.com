---
date: "2009-01-16T16:45:52"
title: "Sun - Week Twenty-eight"
tags: ["ipv6","kvm","sas","t5120","v210","v240","windows"]
categories: ["industrial year"]
---

This week took forever to finish!

The week started off with a couple of light jobs. I replaced a fan in a V210 and racked up an engineers stable system (a V240).
<!--more-->
Tuesday was spent catching up on some work from the week before.

Wednesday everything went bang! I had started to tidy the cable store, but seeing as we were two men down, I came out and worked on reducing tickets on the queue. I went over to GMP02 and readied a customer system for an engineer, this wasn't too hard, but took a while to get console connections to work.

Thursday was spent working on a ticket I had picked up on Wednesday, a ticket from a formidable engineer, one which James had started, be we were a James short! It required taking two T5120's and connecting them to a single storage array. This would have been fine it it were an Ethernet connection, or even fibre, but he wanted it all stuck together with SAS cabling. This cabling doesn't patch around the lab, so I shifted the three machines into a rack where there was space and cabled them all up there.
I also set up a 'generic' x86 box for an engineer. He wanted a box set up in GMP02 which would allow him to install Windows \*shudders\* NT4. I set the box up and left the rest until Friday.

Friday morning came and I had a cabling job in the lab to do before fiddling with the NT4 box. It was just to install an HBA and connect a host up to some storage.
Friday afternoon came and I moved my desk as Paul had asked me to move into the main section of the room. I then set about getting a KVM set up for that Windows NT4 box...It wasn't easy, first I didn't change the KVM settings, so it didn't like being on a different network, then I got the default router wrong, so I spent ages setting up another box so I could use IPv6 to connect to the KVM and change it. I managed to get all this done, then missed the bus home! Had to wait 45 minutes for the next one.

I was glad when the weekend came!

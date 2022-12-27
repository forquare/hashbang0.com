---
date: "2008-08-15T18:13:12"
title: "Sun - Week six"
tags: []
categories: ["industrial year"]
---

So, as you know from my other posts, I have been off to London and Aberystwyth this past week, so was only in work for Friday.
<!--more-->
We had a busy day, I picked up a number of tickets, one was to set up three customer machines, which took longer than anticipated.  Robin had set up an area for customer machine which is great, I set up the three machines and hooked them up.  However, after looking at them, two out of the three console lines didn't work.  After an hours trouble shooting, I still couldn't get the last one to work, so I handed them over to the engineer looking after them and said to him that if anything needed doing, I was there.
I also picked up a ticket regarding another server.  One of our German engineers wanted a specific server type.  We had one that he had booked, but he couldn't get DHCP or networking to work, that would be because all our machines have static IPs, I reinstalled Solaris, and everything seemed fine, it net booted fine, I could ping servers and indeed google.com fine.  So what was the problem?  I dived in back to the ticket and discovered that he was installing Red Hat, so I went ahead and net booted using Red Hat, and here is the snag!  Red Hat wants to use DHCP to get some stuff during the install!  So I had to look up in the NIS hosts table, the machine and find what it's given IP was ,I posted this back to the engineer and closed the ticket, I've not heard anything back just yet...

That was about it!  I got home, found the lights were still not working downstairs, so called Luffs and they said they would sort it, and I made home-made chips, steak and peas...Was nice :P

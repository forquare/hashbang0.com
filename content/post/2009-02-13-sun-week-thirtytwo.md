---
date: "2009-02-13T16:40:04"
title: "Sun - Week Thirty-two"
tags: ["t2000","x2200","x4250"]
categories: ["Sun"]
---

After having a nice week off, I come into work on Monday to find myself with a ticket stuck on my queue and a meeting at 10am about said ticket...
The ticket is to do with a new feature I'm going to be adding into our inventory database. It will basically look at how long a user has booked a system for, then email them if needs be.
I spent some time looking at this project before picking up a ticket on my queue, and was also given a ticket.
The first ticket was just to check to see why a machine wasn't accessible, neither the SP or the host was pingable. I looked at the machine and the power leads had slipped out the back but just a little...But enough to puzzle me at first glance...
The other ticket was given to my by Tom. An Indian engineer was asking for an array and a SCSI card to be moved from a T2000 to an X2200. I did this and got endless questions until the end of the day.
After work I decided it was time to hit the gym!

Tuesday was a little more interesting.
I spent the morning installing some X4250's for an engineer, he had eight, but I only had two rack mount kits, so I racked up two of them leaving enough space to fit one between them and one could sit on top.
I cabled them up and gave them customer entries into out booking system, added them to NIS+ and all the other usual stuff.
Another ticket was to just power cycle a system's SP...
Another SP wasn't accepting SSH connections. After reading the documentation for the SP, I used the serial port to access the SP, then ran:
set /SP/services/ssh state=enabled | disabled
Which sorted it out nicely...

Wednesday I spent working out what the X2200 I set up on Monday didn't want to work, I didn't dwell on it as William was in and I needed to talk to him about the inventory database and how it worked. He talked me through the structure and basic Sybase commands.

Thursday was spent tying up loose ends, and I went to the gym after work!

Friday I helped an engineer with net booting a machine remotely. I showed him how to access the KVM on the SP and net boot it. He was still having problems so I did it manually, but at least he knows now.

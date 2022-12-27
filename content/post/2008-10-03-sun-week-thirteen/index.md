---
date: "2008-10-03T17:15:05"
title: "Sun - Week Thirteen"
tags: ["bill bailey","clive king","console server","enotty","liveupgrade","multihead","royal albert hall","solaris","sunray","ultra 80"]
categories: ["industrial year"]
---

Unlucky for some, will it be for me? As you possibly guess, I write parts of entries throughout the week and publish on Fridays, so at the time of writing, I have no idea if it's going to be a good or bad week, it's 11:50, Monday morning :P
<!--more-->
So Monday starts off with me cycling in, sitting down to breakfast then being interrupted because a fibre cable won't come out of it's socket. I went down to the lab where me and the engineer pulled it out and I took the cable upstairs to bin (must do that sometime actually). While getting ready for the day [Clive King][1] popped into the office! Not sure why he's here, but he was checking out some of the M series machines...I'm pitted against the same problems that were pestering me last week, as well as a problem with an [Ultra 10][2] that is pingable, but won't net boot. I made a new group for a multihead SunRay :D
`utmhadm myName -d 2x1 -p 0123456789ab -l 0123456789ab,cdef01234567`
I spent the afternoon fixing an [Ultra 10][3], the problem was about lab tool 'js\_config' not being properly configured for the machine. I also had an [Ultra 80][4] to look at, it didn't want to boot, so after rearranging the CPU's, I concluded it was a duff system board, so another one of them is on order...
As I cycled out of the campus, a squirrel, obviously daring death, leapt out in front of me, it's tail inches away from my front wheel!

I woke up this morning and went down to the shower finding the kitchen in a mess. While making lunch I found that my tuna had been eaten and that the foil I had bought had been all screwed up. Coupled with the fact that I had forgotten my Sun badge today, I wasn't off to a good start.
At 10:30 I had a meeting with Paul about our console server (enotty), it is still on Solaris 10, the original release! I'm about to do some testing on an [880][5] so I can simulate a [LiveUpgrade][6] and get to know any pitfalls I might encounter.

Wednesday morning started with me replacing the [Ultra 80][7] system board,only to find that it was faulty, so had to replace it with the old system board, send the new one back and request a new board. The system was very different to a normal 'PC'. The RAM was really weird, a little switch rocked left or right to release the DIMMs, the CPU's have a couple of handles on (like DIMM slots), when pulled they unlock the CPU and slide out. The new system board wasn't powering up. I whittled this down to the bottom slot not wanting to be populated with a CPU. Alas, after taking the bottom CPU out, there was still no output on the screen.
During the afternoon I tried to get to grips with the LiveUpgrade stuff, but I was asked to take up another task which involved trying to get a Minnow to work. I've got no idea what I'm doing, everybody seems to think I do, everyone else seems to know a little more than I do too...Didn't get it to work, will leave until tomorrow.

Well, I think this is an unlucky week. I was woken up last night by house mates coming in at 2am, with some girl that just screeched, she ended up sleeping with one of them...They made a racket until 4am, then I finally got some sleep...
The morning at work hasn't been much better, I've been working with this Minnow still, I think I'm trying to trouble shoot problems with an HBA, as now I'm using a unipack and still getting errors....Just swapped the SCSI cable from the upper port on the HBA, to the lower port, and it seems to recognise the disks now! Just got to wait and see whether the engineer is happy now :P Lunch seems in order...
The afternoon was spent trying to get some last few things tied up before home time! This included more work on that blasted [Ultra 80][8]! Still not fixed :(
After work I met up with some of the guys who work on the floor below us, me and Tom went to their house just over the M3 for a few games of cards and some pizza, was a great night :D

Friday starts with estale, one of the Sun Ray servers, crashing...Still not sure why, but suffice to say, it dragged my session down with it...Ho hum, it's been a very busy morning due to people trying to solve this, and people submitting more tickets than usual...I'm sure they heard we were having problems and thought they'd give us a challenge! I've power cycled a machine from the SP, removed a booking and tried to get a switch to work. The switch wouldn't ping or give us a console connection, so I've passed it over to Tom who might be able to do a little bit more! Jobs for this afternoon include making a custom jumpstart image...Oh, I also booked tickets for me and Faye to go see Bill Bailey in a couple of weeks time :D [Clicky][9] Perhaps more importantly; we're going to be in the Royal Albert Hall :D
The afternoon has been quite full. I finally fixed that problem with the [T2000][10] as a node, I just took out all the cards, reinserted them and voila! It worked! I have also been working on getting the customer jumpstart image to work, I've not advanced much...But hopefully I'll get it done by the end of Monday. I've also been postbooking some machines, this has so far just been making sure hosts are alive, then jumpstarting them with Solaris 10 Update 4. Early next week I shall look at making sure they don't have extra cards or network stuff attached to them.

  [1]: http://blogs.sun.com/clive/
  [2]: http://sunsolve.sun.com/handbook_pub/validateUser.do?target=Systems/U10/U10
  [3]: http://sunsolve.sun.com/handbook_pub/validateUser.do?target=Systems/U10/U10
  [4]: http://sunsolve.sun.com/handbook_pub/validateUser.do?target=Systems/U80/U80
  [5]: http://www.sun.com/servers/midrange/v880/
  [6]: http://www.sun.com/software/solaris/liveupgrade/
  [7]: http://sunsolve.sun.com/handbook_pub/validateUser.do?target=Systems/U80/U80
  [8]: http://sunsolve.sun.com/handbook_pub/validateUser.do?target=Systems/U80/U80
  [9]: http://tickets.royalalberthall.com/season/production.aspx?id=12967&src=t&monthyear=10-2008
  [10]: http://www.sun.com/servers/coolthreads/t2000/

+++
date = "2009-01-30T18:56:31"
title = "Sun - Week Thirty"
tags = ["tape library","v240","x2100"]
categories = ["Sun"]
+++

Week thirty! Wow, doesn't seem so long when you say it like that...It's been seven months!

Monday started out quite slowly, we had out afternoon meeting in the morning to talk to one of our Australian friends, but he wasn't able to make the meeting. Monday went slowly with not much going on.

Tuesday I picked up a ticket which asked for a specific HBA and array to be passed from one machine to another and for some tests to be carried out. After doing some prep work for most of the day (getting machines into a good state), I realised that I would not be able to carry out the plans as the engineer (in India) had not told me how he wanted the setup wired.

Wednesday morning came with an email from the engineer, I sorted some more stuff out but didn't get far with it. I fixed the console to an x2100 by defaulting the BIOS back to defaults which fixed serial redirection.

Thursday I had a tricky case. I was asked to set up a tape library and hook it up to a v240. Not difficult in itself, however the two items were around 15 meters away form each other, on opposite sides of the lab and needed to be joined via SCSI. I ended up moving the v240 :(
Next the tape library wouldn't recognise the drives. Or rather the v240 wouldn't, no matter what I tried or how I wired it, I could see the robot, but not the drives :( I ran over the three hour limit on this ticket and had to leave it for Friday.

Friday morning and I started looking into this tape problem again. In the end I got an engineer to look at it. We couldn't figure out what the problem was, we had tried changing terminators, changing how we wired everything, but still nothing. In the end we decided to flash the firmware on the library, however someone interrupted the update and bricked the library :( We have someone coming in next week to fix it.
When I told my engineer about this, he just asked me to hook up a small, potable tape drive instead, as that would be fine. I was fuming, all this work and a small thing would have sufficed in the first place

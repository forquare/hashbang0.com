+++
date = "2008-09-05T18:27:37"
title = "Sun - Week Nine"
tags = []
categories = ["Sun"]
+++

So, two months on and we have moved from knowing nothing, to knowing a bit more about what we're doing.

Monday started with panic, one of our SunRay servers had been upgraded, and now things didn't work...Though we think it was an ITOPs issue, as we didn't fix it...The server is question has been upgraded to a later version of Navada.
The afternoon was littered with tickets and went very quickly.

Tuesday morning started with looking at some tickets I picked up last thing on Monday, I had an [880][1] to look at, this now requires me to test 32 ports on an etherlite, and then potentially label up said 32 ports and cables, then pull them so I can reset the etherlite (just resetting the etherlite with cables plugged in will send a break to all the attached machines).
The afternoon was spent trying to get a [v880][2] to work, it would appear that the RSC is flakey...Might be a case that it needs a new one, or it may be that the firmware is incompatible with something else...I also shifted a A5200, which was really heavy, to the overflow lab.

Wednesday morning was hectic; Liam and James were both out of the office, and there was a number of tickets on the queue. We all took a number each and set to work. My first job was to talk to Paul about some of the ticket details. Then I spent 20 minutes talking to Michael Clarke about a [25k][3] problem, which turned out to be more of a problem with [NIS+.][4] I am to order in some new parts for an [ultra 24][5] workstation as the one in question is overheating. I have just moved the Minnows to GMP02, and am waiting for a guy to come and see me about them...

Thursday came and went, with more tickets coming and going. More looking at machines that seem dead, and rebooting them.
Faye came to see me for lunch :D
After work, we went to the Dolphin for a pint, and we were all sad to hear that it will be closing shortly, the to a restaurant called 'so asia', this was all because Michael Clarke had come back for a day. We went to 'The four horseshoes' and won the pub quiz after arriving late.

Friday mornign started with a conversation with Paul on the bus! He had checked his car into a garage and had caught the bus. When we got into work Liam showed me how to kill off sunray sessions as he needed to use his main card and had a duff session trapped on it.
I also looked at an old 65x which seemed to have a memory problem. Looking up some docs I managed to cure this. The machine is now running Solaris 10 update 4 quite happily. I also had a [v240][6] which didn't appear to be alive. After a reboot and fresh install, all is good.
This afternoon I spent a lot of time finding a camera. The camera was so an engineer in Australia could see some LEDs on the box he had booked.
I got my Sun Ray today, however, I've forgotten my PIN number! Silly me! :(

  [1]: http://www.sun.com/servers/midrange/v880/
  [2]: http://www.sun.com/servers/midrange/v880/
  [3]: http://www.sun.com/servers/highend/sunfire_e25k/index.xml
  [4]: http://en.wikipedia.org/wiki/NIS%2B
  [5]: http://www.sun.com/desktop/workstation/ultra24/
  [6]: http://www.sun.com/servers/entry/v240/

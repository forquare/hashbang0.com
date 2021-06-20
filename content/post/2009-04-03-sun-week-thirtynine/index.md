---
date: "2009-04-03T16:39:48"
title: "Sun - Week Thirty-nine"
tags: ["april fools","console","customise boot image","devalias","Java cafe"]
categories: ["Sun"]
---

Monday morning is here, we are an hour ahead of last week with the clocks going forward. It seems really sunny today, shame that my day starts off with unblocking the toilet, yuck.
Anyhoo, I get into the office as usual, grab a coffee from the Java cafe and spend most of the morning setting up some of the French kit. I've set up console entries in JLT, and all console connections appear to work. I've just got to set up IP addresses for the kit and their SP's, configure all the SP's to work on this network and boot net the hosts. However I must configure boot net for any hosts that are booked, but not jump start them...Something which is very tempting!
The afternoon was spent in a meting, and finding three machines. I want to test something out: we have had complaints that from Solaris 10 machines, people can't access one of our share through /net though Nevada machines seem to be unaffected...I have three v210's which I have installed Solaris 10 update 4 and 6, and Nevada build 110 on, I have found that I can reproduce the issue, just a case of finding out why!

Tuesday came and went with me looking over the machines again, and looking into some problems as to why an engineer couldn't get console to work.

Wednesday, 1st April...April fools day...
Again, I mainly looked at kit, labelling them up etc today. I also fixed an issue in the states where an engineer couldn't boot net - install a machine, because they had changed the devalias' in OBP sigh\*

Thursday morning was spent fixing some console connections to those French machines, they are doing my head in! There are still two to fix :(
The afternoon was more exciting, to an extent....An engineer wanted to customise a boot image, I had done this on Solaris 10 update 5, but in update 6, the game had changed....
We had to unpack the miniroot, apply the patch, the repackage the miniroot...I'll post the steps later in a separate entry I think...

Friday! End of the week! I spent today catching up on some other bits and bobs, as well as sorting a couple of bookings for engineers.

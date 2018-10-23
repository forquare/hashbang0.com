+++
date = "2008-12-12T16:40:31"
title = "Sun - Week Twenty-three"
tags = ["etherlite","LX50","Solaris 9","Sun Ray","v20z"]
categories = ["Sun"]
+++

Monday starts considerably well seeing as Paul is out this week. Tom is also on a course so we are two people down.
I spent a large part of the morning over in GMP02 looking at some machines which were overheating. The A/C seems to have gone wrong over there and the machines are suffering. After I fixed some machines temporally I came back over to the office and it was almost lunch time.
After lunch I went down to the lab to look at connecting up a couple of systems. It wasn't especially complicated, but it took me a while as I needed to install some extra cards and whatnot.

Tuesday I picked up a ticket asking about boot netting an LX50 with Solaris 9. The problem isn't one I'd seen before. But after getting past the first hurdle, we came across the wall. An error given by the installer claims that it can't find valid media...I asked the guy to book a system in a different lab, and I'm now looking at getting my hands on a V20Z to test to see if they have the same error.
I fixed a console connection too, I replaced all of the wiring, changed the ports on the etherlite, and the things that was faulty in the end was someone had used the wrong db9 connector...\*sigh\* Oh well, it works now!

Wednesday wasn't particularly busy, with me fixing some tickets here and there.

Thursday saw me writing a script to take several input files and join them into a single file. This also made a calculation about how many entries were duplicated and how many times they were duplicated.

Friday saw me setting up a new test Sun Ray server. We are having a lot of problems with peoples XServer's dieing. I managed to pin point the problem down to a single event. This will hopefully help someone in the know.

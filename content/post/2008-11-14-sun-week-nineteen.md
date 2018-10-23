+++
date = "2008-11-14T22:30:01"
title = "Sun - Week Nineteen"
tags = ["6000 chassis","6048 chassis","back to back","blade chassis","gDoc","navada","storage","T1","v20z"]
categories = ["Sun"]
+++

After spending most of Sunday cooking meals for the following two weeks, I came in to find that it was fairly quiet. I spent Monday looking at some disk drives, in meetings and looking at gDoc. I also looked at the ticket with the Neptune card. I now needed to put two systems back to back, I corresponded with the engineer to book another system.

Tuesday morning we had a meeting with Paul and David to discuss how things were going, we have planned some more TOI's about some of the finer points here and there.
I picked up a ticket from an engineer in the Czech Republic, he was trying to install the latest [Navada][1] build on a [V20Z][2], but he couldn't...I have tried it also, but couldn't get a lab box to install...Most strange...
In the afternoon I had heard back about the back-to-back systems. They were at opposite ends of the lab, typical! I patched them through to each other and reported to the engineer what the configuration was complete.

Wednesday I looked at the Neptune ticket, the one I had hooked up two systems back-to-back the night before, the engineer claimed to not be able to see the connection. I went down to the lab only to find that someone had undone all my patching. Everything was gone. I hooked them back up and told the engineer what had happened.
I also picked up two more tickets; one was to install a card in a T1, then hook it up to some storage, more patching across the lab! The other was to transfer a blade from a [6048][3] chassis, to a [6000][4] chassis and install some PCI fibre cards. I had to rename the system to make the blade work, after I had completed it I reported back to say it had been done.

Thursday morning started with the two tickets from yesterday requesting more facilities. The first one from yesterday wanted me to patch the T1 and storage through a brocade switch, this was simply some repatching which was nice and easy.
The second ticket asked for some storage, the guy had booked an array, but it was only an expansion tray, so didn't have fibre connectivity. After hunting around I found something a bit more suitable and hooked them up, patching the two machines together.
James asked me to plug another port of e2big (our NFS server) into a black diamond switch, so I went down to the production cage to do that, it was fiddly as the cables run up near the roof, and being as short as I am, even the step ladder didn't get me up high enough. I did it in the end, with some careful throwing of cables and retrieving them using a very long screwdriver...
I spent the afternoon trying to create some LUNs for an engineer. For this I needed to add the storage controllers to a management suite, this turned out harder than it needed to be...Out [M8000][5] had decided to use one of the IP addresses, so after half sorting this out (we gave the IP a false hostname and created a ticket for Steve to sort it out, then assigned a new IP to the controller), I re-registered the controller and started making pools and volumes...We shall see what the result is on Friday, as I can't get connected to the host at the minute...

Friday morning I finally sorted out that storage. Then I focused on getting some more of gDoc done.
After lunch I looked at all of our recycled machines for Paul, updated the sheets etc...

  [1]: http://opensolaris.org/os/downloads/
  [2]: http://www.sun.com/servers/entry/v20z/index.jsp
  [3]: http://www.sun.com/servers/blades/6048chassis/
  [4]: http://www.sun.com/servers/blades/6000/
  [5]: http://www.sun.com/servers/highend/m8000/

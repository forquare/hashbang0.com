+++
date = "2009-03-27T16:10:15"
title = "Sun - Week Thirtyeight"
tags = ["15k","25k","5100","firmware","js_config","nevada","OpenSolaris","sparc","starcat","T1000","tftp","x4100"]
categories = ["Sun"]
+++

Monday I was back in the office, Steve was out coming home from a weekend in Aber and James had started a week of touring the UK.
I picked up a few tickets and knuckled down to get them done. I had one which was to simply check if some connections between a storage array and a host was OK and to provide feedback on how they were configured.
Another was to do some testing on our lab tool "js\_config", the developer has updated the current version and logged about 25 tickets to test the new and old features out. I picked one which had me installing a SPARC machine with the latest Nevada build, everything seemed to go OK.
I spent the afternoon moving a cryto card from a stable system and putting it in to a T1000, and back again, and then back again, finally placing it back in the stable system...I still have the original crypto card from the T1000 on my desk actually...

Tuesday came and it felt like Monday again :(
I spent a lot of time in the very warm GMP02 lab working with the 15K over there. I had picked up a ticket which involved getting a starcat domain up running with similar hardware to a customer setup.
I found a board with the same CPU's, and changed the memory configuration to match too. I then had problems booting the domain, but I needed to leave it as I had other work that was more urgent.
I had to detach the host from the array and put it back again. I also fixed a problem with the host which stopped it from jumpstarting.

Wednesday felt a lot like Thursday...
I gave the morning to the 15K, it wasn't booting, then when it did boot it wouldn't plumb the network card. I figured this was because I reconfigured it to use a different IO boat (I had swapped the boats to match the customer config more closely). Firstly, I looked at the RAM, and found I had configured it wrong, I put this right and moved onto the whole network thing...I decided to net boot it as OBP could see the network. This worked and I handed the domain to the engineer, I also set up a place for him to put a Flash Archive which was taken from the customer system.
I also did some more things with that host and array.
There was some customer 10g fibre cards in our 25K, so I pulled them out and gave them back to the relevant engineer, I've still got to find some replacements...
An engineer came to me asking why his desktop wouldn't boot, it kept timing out. We finally whittled this down to the fact he was on the wrong network, he has been moved over now and all is OK.
I also gave blood! The blood people were at work so went to donate.

Thursday is here and I'm sure it's supposed to be holiday time...
I woke up this morning feeling really cold and had a horrid neck ache, to make matters worse the shower was cold as someone had turned off the heating. I felt generally miserable and unmotivated all day :(
I mainly did more work on the host and array, adding remote power to the array which would mean I have less to do with it later in the week/next week.
I've got to look at James' racks from France this afternoon, joy of joys! they aren't full, so hopefully between today and tomorrow I'll get some done!
I've know I've really got to keep this blog more up to date, I've got a load of OpenSolaris stuff to blog about, some important lessons to be learned and stuff like that...Will try and do that tonight...

Well, it's finally Friday! It's been an odd old week.
Today I spent the morning documenting some of the stuff from the French lab, I've moved all the machines into out lab group, and have determined whether they have asset and serial numbers, I've collected missing serial numbers and need to find some asset stickers...
Lunch was spent updating this entry actually...And sprucing up my CV for position I'm hoping to get next year. More of that if I get the job ;)
After lunch I went back to documenting the servers again, it's a horribly boring task, and fiddly, one slip up and you've got to start again...Maybe that's just my 'accounting' though.
I also managed to get an x4100 to boot [OpenSolaris][1] 2008.11! I had to flash the firmware as when I found the machine, it had really old firmware, and no disks which was odd...Though I learned how to set up a TFTP server :D

  [1]: http://www.opensolaris.com/

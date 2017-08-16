+++
date = "2009-05-01T17:14:40"
title = "Sun - Week Fortythree"
tags = ["10 gigabit","10Gb","64-bit","BIOS","Java","PCI option ROM","POST","raid","t2000","windows","x4100","x4150","x4250"]
categories = ["Sun"]
+++

Monday starts off OK. Coming into work to find some tickets on the queue and some work to be done from last week.
My main ticket is setting up an [x4100][1] over in the 02 lab for an engineer who wants to install VM's. There is a problem with this, he wants to use [Windows][2] and whenever someone says that, there are always groans, why? Because Windows == licensing. He wants to use MSDN licenses, however, he isn't the only user of the systems-to-be, so he needs to use volume licenses. We have an old VM machine which has valid volume licenses on, but I can't access the VM files so I can't transfer them...

Tuesday was a terrible day, it started terrible and ended terrible. I woke up late, to Tom asking if I was ready to go at 8:30. After telling him I'd just got up and rushed to get ready and get the next bus.
When I got in the ticketing system was down, when it came back up one of my tickets had gone from being a normal priority to a high priority...The ticket was to set up two Intel boxes, an [x4150][3] and an [x4250][4], each with a 10 gigabit Ethernet card. This should have been a really easy ticket. However it turned into a nightmare.
Installing the 10Gb card gave me PCI option ROM errors during POST, I wouldn't usually care, but this meant the BIOS couldn't see the RAID card and so couldn't use the disks. After flashing the BIOS and trying the on-board disk controller and still getting no where, it was 6:25pm and I decided enough was enough.

Wednesday was significantly better than Tuesday. I got in and disabled PCI option ROM for the PCI slot the 10Gb card was in, this allowed the BIOS to see the RAId card and the disks, I booted from the disks and used \`ifconfig -a plumb\` to see all the interfaces and found the 10Gb card! I also managed to install 64-bit Java to a shared directory (on a SPARC host!) and get it to work on an x64 system (simply install on an x64 host, then copy the files over).
The afternoon was mostly consumed by a meeting with our new boss...All the labstaff from around the globe jumped on a call which was just as exciting as I'd predicted...

Thursday didn't start well, I woke up a little later than usual and felt disorientated most of the day, I think it's a lack of sleep...
The morning was spent assigning some IP's for an engineer and set up a [T2000][5] sc for him.
The afternoon was spent troubleshooting a 15K domain. Solaris would ping the local production server, but OBP couldn't see any network activity...The interface with the cable in told me that I should check for a problem with the cable, and I did. Moving the cable around made this message follow the cable to different interfaces and using a known working cable didn't make a difference. It's very weird, it's like BIOS not seeing your disk, but Windows being able to see and access it fine...In the end I delved into /etc/path\_to\_inst and copied the device path, under OBP I then did:
boot /pci@9c,700000/pci@1/network@0 - install
This seems to have worked and the domain is net booting as I type this!

Friday was spent fixing console access for an engineer and finding a PSU for a customer system. We also moved some old racks into the campus data center, they were very kind to let us use some of their space. The old racks contain machines which have been end of service lifed, but may still be called up if a big customer offers to pay the right amount of money.

  [1]: http://www.sun.com/servers/entry/x4100/
  [2]: http://www.microsoft.com/windows/
  [3]: http://www.sun.com/servers/x64/x4150/
  [4]: http://www.sun.com/servers/x64/x4250/
  [5]: http://www.sun.com/servers/coolthreads/t2000/

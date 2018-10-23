+++
date = "2009-01-20T00:49:07"
title = "My new IBM Thinkpad X32 and wireless"
tags = ["2008.11","IBM","OpenSolaris","ThinkPad","Wireless","X32"]
categories = ["OpenSolaris"]
+++

Today I took delivery of the four year old IBM Thinkpad X32.  With it's 1.8GHz Pentium M, 1GB of RAM and it's "massive" 40GB HDD ;) I thought it was an excellent machine to use as a portable OpenSolaris machine.I used the USB stick I had prepared last night to boot the live image and do the install, within the first hour James had messed with the driver file and stopped the machine from booting \*sigh\*.
A reinstall later and I was up and running OpenSolaris 2008.11!

At home everything was still running smoothly, the battery had lasted the travel from work (it doesn't suspend yet...) .  My only problem was that it didn't like the WPA2-psk wireless in the house.  I had two choices:
1) Screw every computer in the house over and change to WEP.
2) Look for a work around.

I opted for 2.  My work around was very simple actually.  Build 105 fixes the compatibility with WPA2, so I added the developers repository to my authority list and live upgraded the machine to build 105.

Now I can happily lye in bed, with my X32, and write this blog entry!

Build 105 seems fairly stable so far...Not that I've tried doing much yet.  Will report findings as I go along :P

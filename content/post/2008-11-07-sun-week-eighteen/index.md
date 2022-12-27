---
date: "2008-11-07T18:26:13"
title: "Sun - Week Eighteen"
tags: ["multihead","sunray","utmhadm"]
categories: ["industrial year"]
---

Monday was spent travelling back from Aber, I arrived back in Camberley at just past midnight
<!--more-->
Tuesday was spent getting some storage trays set up for an engineer. It just comprised of setting up two trays really, there was thought about getting me to hook the systems up, but the engineer decided to do the rest himself.
I spent a portion of the afternoon postbooking some systems in th 02 lab as they were appearing dead. I brought two of them back to life...One still needs my attention.

On Wednesday I completed two [SunRay][1] related tickets. One was to tear down a multi-head group, and another was to replace two DTU's (DeskTop Unit) which had broken. They caused a peculiar problem which meant that the mouse froze. I replaced the two units with a couple of spare and still have the old ones sitting on my desk for me to test...I had to tear down the old multi-head group and make a new one:

```
utmhadm
<..List of multi head groups..>
su -
utmhadm -d oldMultiHeadGroup
utmhadm -a newMultiHeadGroup -g 2x1 -p primaryMAC -l primaryMAC,secondaryMAC
^D
```

Thursday was spent corresponding to an engineer who wanted an unspecified host linked up with unspecified storage. After making him book the various pieces of kit, I started setting up the infrastructure. Basically just using a fibre connection from a host ([x4100][2]) to a storage array ([StorageTek 2540][3]).

On Friday I set up a new KVM unit. A 1U tray which looks a bit like a laptop, cables come out of the back and you can hook them up to hosts to see whats coming out the VGA port.
I was also given a Neptune card by Paul to plug into a machine, along with the card came a ticket which involved just plugging the card in at this point...More next week I suppose...

  [1]: http://www.sun.com/software/index.jsp?cat=Desktop&tab=3&subcat=Sun%20Ray%20Clients
  [2]: http://www.sun.com/servers/entry/x4100/
  [3]: http://www.sun.com/storage/disk_systems/workgroup/2540/

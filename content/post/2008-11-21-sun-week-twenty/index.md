---
date: "2008-11-21T11:55:43"
title: "Sun - Week Twenty"
tags: ["live upgrade","luactivate","lucreate","lumake","luugrade","sunblade","SunRay"]
categories: ["industrial year"]
---

Monday starts off quietly, with fewer tickets than I expected...I progressed a little with some gDoc code, you can now call up the GUI form the command line with some arguments and the app with automagically search for those words.
<!--more-->
I have also been looking into live upgrading from Solaris 10 to Solaris 10 update 6. I think that I have finally understood how to create a new Boot Environment (BE). I am just waiting for the machine (an [880][1]) to copy the current BE into a new one. I will upgrade the new BE while the system is still running. Then when I know that it is OK, I will simply restart the machine and it will boot into the upgraded environment...

```
bash-3.00# df
/ (/dev/md/dsk/d0 ):14587530 blocks 1118064 files
/devices (/devices ): 0 blocks 0 files
/system/contract (ctfs ): 0 blocks 2147483611 files
/proc (proc ): 0 blocks 29950 files
/etc/mnttab (mnttab ): 0 blocks 0 files
/etc/svc/volatile (swap ):137777776 blocks 4805821 files
/system/object (objfs ): 0 blocks 2147483504 files
/dev/fd (fd ): 0 blocks 0 files
/var (/dev/md/dsk/d10 ):12278616 blocks 730732 files
/tmp (swap ):137777776 blocks 4805821 files
/var/run (swap ):137777776 blocks 4805821 files
/home/bl222517 (egmp-home1.uk:/export/home1/04/bl222517):167982268 blocks 29777260 files
/usr/dist (mf-egmp-01,mf-egmp-02:/usr/dist,rpedist:/export/dist):17190092 blocks 3738502 files
/usr/local (e2big:/export/local/sparc):677065750 blocks 677065750 files
/usr/local/share (e2big:/export/local/share):677065750 blocks 677065750 files
/share/lang (e2big:/export/lang):677065750 blocks 677065750 files
bash-3.00# metainit d111 1 1 /dev/rdsk/c1t5d0s0
d111: Concat/Stripe is setup
bash-3.00# metainit d110 -m d111
d110: Mirror is setup
bash-3.00# metainit d121 1 1 /dev/rdsk/c1t5d0s3
d121: Concat/Stripe is setup
bash-3.00# metainit d120 -m d121
d120: Mirror is setup
bash-3.00# lucreate -c s10 -m /:/dev/md/dsk/d110:ufs -m /var:/dev/md/dsk/d120:ufs -n s10u6
```

Just some goop for you all...

Tuesday was more of the same...I finally managed to Live Upgrade my [880][2]. In fact I updated it twice...Once from Solaris 10, to Solaris 10 Update 6, then to Nevada build 102.
I basically did what I posted yesterday, but prior to that I installed the new Live Upgrade packages from the CD. And I also installed a shed load of patches. I made sure the patches were installed so nothing had missing dependencies...Then realised patchadd -M \`pwd\` patches will sort that out for you...ho hum!
After doing this I just did luupgrade pointing to the new BE and the CD, and it went and did the upgrade, and I was able to use the machine as I always have. After that it's just a case of pointing luactivate at the BE and rebooting the machine with init 6.

Wednesday I wanted to make sure I had this LU thing nailed, and so set about doing it one last time and documenting what I was doing.
I spent the afternoon looking at some tickets, I hooked up a multipack to an [ultra 40][3], and replaced a [T5120][4] system board after an engineer had done some testing (no, he hadn't broken it, he was testing a new board or something).

I came into the office on Thursday finding David rushing towards me. When I had replaced the system board from the [T5120][5] yesterday, I had forgotten to take off the address module (which holds the MAC address and possibly other things). So I ran off the goods in and retrieved the package before it had left, replaced the module and gave the package back. I then ran up to the office and looked at a dead machine. I haven't managed to get it working yet, but I'm going to keep thinking.
I worked through lunch so I could play with the [V880][6] more, and also looked at my [SunBlade 150][7] which I hope to install the SunRay server software on.
I also replaced a failed disk into the array used by our NFS server.
Blog updates: I looked at all the last nineteen posts and put links in them to point to various machines and software sites. And machine I mention in my blog, I will try to link it to the relevant homepage for it, the same goes for any interesting software.
I also posted an entry about SunRay keyboard shortcuts

  [1]: http://www.sun.com/servers/midrange/v880/
  [2]: http://www.sun.com/servers/midrange/v880/
  [3]: http://www.sun.com/desktop/workstation/ultra40/
  [4]: http://www.sun.com/servers/coolthreads/t5120/
  [5]: http://www.sun.com/servers/coolthreads/t5120/
  [6]: http://www.sun.com/servers/midrange/v880/
  [7]: http://www.sun.com/desktop/workstation/sunblade150/

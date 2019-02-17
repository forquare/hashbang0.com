---
date: "2008-10-10T16:59:17"
title: "Sun - Week Fourteen"
tags: ["drivers","fibre cards","Floppy disk","jumpstart","jumpstart profile","LX50","mirror","multiple mirrors over jumpstart","patchadd","Solaris 9","V65s","windows"]
categories: ["Sun"]
---

Monday starts off with me looking into this jumpstart image. It turns out to be really easy! All I needed to do was to find the Solaris 10 Update 4 image, this had a file called setup\_install\_server (or similar), I just used this executable file to install the image to another directory. After this it was a case of using the patchadd command:

`patchadd -C  `

And this installed that patch onto the image for installation at install time! A bit clever or what!?
The afternoon was spent looking at mirroring a machine at install over jumpstart...I didn't get it to work :( But I shall keep trying!

Tuesday was spent looking into why I couldn't jumpstart an X86 box with Solaris 9 update 8...I tried various machines in our lab, as well as other machines around the world, some labs don't even have an image that old!
The [V65s][1] and the [LX50s][2] that I tried either didn't have rpower configured, or were just being a pain. Alas, I didn't finish the task by the end of the day :(

Wednesday morning was partially spent on yesterdays problem, but I was also asked to help set up an [X2200.][3]
The afternoon was spent trying to install \*Windows\* onto that server :| It felt wrong and awful...

Thursday starts with me continuing the install of Windows....Apparently it doesn't know how to do SATA raid :roll: Oh well, out comes the floppy disk so I can load drivers on!
The afternoon was spent doing some tickets. One involved moving some firmware versions form one place to another and reorganising some of firmware files. I also got the chance to look at my [880,][4] which I'm using to try and practice LiveUpgrades. I'm currently learning about the JumpStart profiles..

.[<img src="http://i9.photobucket.com/albums/a55/forquare/blog/Picture1-1.png" width="80" height="78" class="alignnone" title="Floppy!" />][5]

Friday morning I spent installing the drivers for the Windows box that didn't understand fibre cards. I then helped David and James set up some storage for the box.
I then went over to GMP02 to receive a package, this contained some fan trays for an [880][6] in GMP02 , so I went upstairs to fix it. I forgot to check the console line, so shut the server down while the engineer was doing work on it...I apologised and quickly went to lunch,
After a slightly elongated lunch I received another email from the engineer who was very good about the situation :)
I then spent the afternoon playing with custom Jumpstarts with my [880\.][7] At the moment it's complaining about unexpected allocated inodes....Here is my profile, it's most probably wrong:

`install_type initial_install
system_type standalone
partitioning explicit
metadb c1t0d0s7 size 8192 count 3
metadb c1t1d0s7 size 8192 count 3
filesys mirror:d0 c1t0d0s0 c1t1d0s0 10240 /
filesys mirror:d0 c1t0d0s3 c1t1d0s3 10240 /var
filesys mirror:d0 c1t0d0s4 c1t1d0s4 10240 /opt
filesys mirror:d0 c1t0d0s5 c1t1d0s5 10240 /mnt
filesys mirror:d0 c1t0d0s6 c1t1d0s6 10240 /tmp
filesys c1t0d0s1 free swap
isa_bits 64
boot_device any update
cluster SUNWCall`

Hopefully I can get something out of it :)

Looking at [this][8] doc, it seems this should work:
`install_type initial_install
system_type standalone
partitioning explicit
metadb c1t0d0s7 size 8192 count 3
metadb c1t1d0s7 size 8192 count 3
filesys mirror c1t0d0s0 c1t1d0s0 10240 /
filesys mirror c1t0d0s3 c1t1d0s3 10240 /var
filesys mirror c1t0d0s4 c1t1d0s4 10240 /opt
filesys mirror c1t0d0s5 c1t1d0s5 10240 /mnt
filesys mirror c1t0d0s6 c1t1d0s6 10240 /tmp
filesys c1t0d0s1 free swap
isa_bits 64
boot_device any update
cluster SUNWCall`

Seems to work!

**Processing profile
- Selecting cluster (SUNWCall)
- Selecting all disks
- Configuring boot device
- Configuring SVM State Database Replica on (c1t0d0s7)
- Configuring SVM State Database Replica on (c1t1d0s7)
- Configuring SVM Mirror Volume on / (c1t0d0s0)
- Configuring SVM Mirror Volume on (c1t1d0s0)
- Configuring SVM Mirror Volume on /var (c1t0d0s3)
- Configuring SVM Mirror Volume on (c1t1d0s3)
- Configuring SVM Mirror Volume on /opt (c1t0d0s4)
- Configuring SVM Mirror Volume on (c1t1d0s4)
- Configuring SVM Mirror Volume on /mnt (c1t0d0s5)
- Configuring SVM Mirror Volume on (c1t1d0s5)
- Configuring SVM Mirror Volume on /tmp (c1t0d0s6)
- Configuring SVM Mirror Volume on (c1t1d0s6)
- Configuring swap (c1t0d0s1)
- Deselecting unmodified disk (c1t2d0)
- Deselecting unmodified disk (c1t3d0)
- Deselecting unmodified disk (c1t4d0)
- Deselecting unmodified disk (c1t5d0)**

**Verifying disk configuration
- WARNING: Unused disk space (c1t1d0)
- WARNING: Changing the system's default boot device in the EEPROM**

**Verifying space allocation
- Total software size: 3014.27 Mbytes**

**Preparing system for Solaris install**

**Configuring disk (c1t0d0)
- Creating Solaris disk label (VTOC)**

**Configuring disk (c1t1d0)
- Creating Solaris disk label (VTOC)**

**Creating and checking UFS file systems
- Creating / (c1t0d0s0)
- Creating /var (c1t0d0s3)
- Creating /opt (c1t0d0s4)
- Creating /mnt (c1t0d0s5)
- Creating /tmp (c1t0d0s6)**

**Creating SVM Meta Devices
- Creating SVM State Replica on disk c1t0d0s7
- metadb: v4u-880h-gmp03: network/rpc/meta:default: failed to enable/disable SVM service
- Creating SVM State Replica on disk c1t1d0s7
- metadb: waiting on /etc/lvm/lock
- Creating SVM Mirror Volume d0 (/)
- Creating SVM Mirror Volume d10 (/var)
- Creating SVM Mirror Volume d20 (/opt)
- Creating SVM Mirror Volume d30 (/mnt)
- Creating SVM Mirror Volume d40 (/tmp)**

After this it seems to go into the installation.

Also, an alarm went off right next to where we work, so most of us had ear defenders on...
[<img src="http://i9.photobucket.com/albums/a55/forquare/blog/Photo8.jpg" width="91" height="68" class="alignnone" title="Ben with ear defenders" />][9]

  [1]: http://www.sun.com/servers/entry/v65x/
  [2]: http://www.sun.com/servers/entry/lx50/
  [3]: http://www.sun.com/servers/x64/x2200/
  [4]: http://www.sun.com/servers/midrange/v880/
  [5]: http://i9.photobucket.com/albums/a55/forquare/blog/Picture1-1.png
  [6]: http://www.sun.com/servers/midrange/v880/
  [7]: http://www.sun.com/servers/midrange/v880/
  [8]: http://docs.sun.com/app/docs/doc/819-6397/6n8dpr9c0?a=view
  [9]: http://i9.photobucket.com/albums/a55/forquare/blog/Photo8.jpg

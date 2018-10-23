+++
date = "2009-03-01T20:36:28"
title = "Beleg-Iâ gets an upgrade"
tags = ["beleg-iâ","mirror","raidz","zfs","Zpool"]
categories = ["beleg-iâ","OpenSolaris"]
+++

It happened a couple of weeks ago now, I've upgraded the storage in beleg-iâ.  It now has 4x 500GB disks!
I have them in two mirrors, one for the boot disk and one for data.


**root@beleg-ia:/share/media# zpool status
pool: rpool
state: ONLINE
scrub: none requested
config:**



**NAME        STATE     READ WRITE CKSUM
rpool       ONLINE       0     0     0
mirror    ONLINE       0     0     0
c4d0s0  ONLINE       0     0     0
c4d1s0  ONLINE       0     0     0**


**errors: No known data errors**





**pool: share
state: ONLINE
scrub: none requested
config:**



**NAME        STATE     READ WRITE CKSUM
share       ONLINE       0     0     0
mirror    ONLINE       0     0     0
c5d0s0  ONLINE       0     0     0
c5d1s0  ONLINE       0     0     0**


**errors: No known data errors**



The above was achieved by doing something similar to this:
# prtvtoc /dev/rdsk/c4d0s0 | fmthard -s - /dev/rdsk/c4d1s0
# prtvtoc /dev/rdsk/c4d0s0 | fmthard -s - /dev/rdsk/c5d0s0
# prtvtoc /dev/rdsk/c4d0s0 | fmthard -s - /dev/rdsk/c5d1s0
# zpool attach -f rpool c4d0s0 c4d1s0
# zpool attach -f share c5d0s0 c5d1s0

First we print the vtoc (volume table of contents) and pipe that to a format command.
The -f was used to force the attachment, I followed instructions from several other sources and all has worked for me.

The 'share' pool is auto-mounted to /share which is cool.  Now with file systems galore using ZFS, \`zfs list\` give me the following output:


root@beleg-ia:/share/media# zfs list
**NAME                            USED  AVAIL  REFER  MOUNTPOINT
rpool                          33.1G   424G    75K  /rpool
rpool/ROOT                     4.79G   424G    18K  legacy
rpool/ROOT/opensolaris         5.42M   424G  2.27G  /
rpool/ROOT/opensolaris-1       4.79G   424G  4.55G  /
rpool/dump                     1018M   424G  1018M  -
rpool/export                   26.3G   424G    19K  /export
rpool/export/home              26.3G   424G    21K  /export/home
rpool/export/home/admin        28.8M   424G  28.8M  /export/home/admin
rpool/export/home/ben          26.3G   424G  26.3G  /export/home/ben
rpool/export/home/ben/backups  20.4M   424G  20.4M  /export/home/ben/backups
rpool/swap                     1018M   425G  31.9M  -
share                           126G   331G    19K  /share
share/media                     126G   331G    27K  /share/media
share/media/iTunes             5.66G   331G  5.66G  /share/media/iTunes
share/media/misc                911M   331G   911M  /share/media/misc
share/media/movies             61.8G   331G  61.8G  /share/media/movies
share/media/pictures            425M   331G   425M  /share/media/pictures
share/media/standup            5.32G   331G  5.32G  /share/media/standup
share/media/torip              4.50G   331G  4.50G  /share/media/torip
share/media/tv                 47.7G   331G  47.7G  /share/media/tv**

So I'm pretty happy with it at the moment.  I'll be looking at upgrading the motherboard next.  Something with 8 SATA channels so I can increase my HDD's and potentially create a RAIDz for 4x 1TB drives...But we'll have to see how funds go...

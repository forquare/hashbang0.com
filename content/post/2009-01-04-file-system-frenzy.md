---
date: "2009-01-04T03:52:34"
title: "File system frenzy"
tags: ["nfs","sharenfs","zfs","Zpool"]
categories: ["beleg-iâ","OpenSolaris"]
---

On my server, I've been looking into having various ZFS file systems, this would allow me to better manage parts of the system later on.

I have so far set it all up as such:

ben@BELEG-IA:/share/media$ zfs list
NAME                                  ** ** USED    AVAIL  REFER  MOUNTPOINT
rpool                                      ** ** 26.1G     431G    72K  /rpool
rpool/ROOT                        ** ** 3.10G    431G    18K  legacy
rpool/ROOT/opensolaris  ** ** 3.10G   431G  2.96G  /
rpool/dump                        ** ** 1018M   431G  1018M  -
rpool/export                       ** ** 640K   431G    19K  /export
rpool/export/home              ** ** 622K   431G    19K  /export/home
rpool/export/home/ben      ** **602K   431G   602K  /export/home/ben
**rpool/share **** **** 21.1G   431G    19K  /share
rpool/share/media                      21.1G   431G    33K  /share/media
rpool/share/media/movies **** **** 21.1G   431G  21.1G  /share/media/movies
rpool/share/media/music        18K   431G    18K  /share/media/music
rpool/share/media/pictures   18K   431G    18K  /share/media/pictures
rpool/share/media/tv                18K   431G    18K  /share/media/tv**
rpool/swap                                           1018M   432G    16K  -

The bold entries are separate file systems I have created using:
zfs create rpool/

For example:
zfs create rpool/share

I then did the following to allow the file system and all sub systems to be shared over NFS:
zfs set sharenfs=on rpool/share

So far I've not set up any sort of snapshotting...

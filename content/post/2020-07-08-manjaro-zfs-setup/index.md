---
date: 2020-07-08T18:38:00
title: "Manjaro ZFS Setup"
tags: ["Linux","desktop"]
categories: ["computing"]
---

At the end of [my last post](/2020/05/31/migrating-my-desktop-to-linux/) I noted that I'd switched from an Ubuntu flavoured distro to [Manjaro Linux](https://www.manjaro.org/).  I've been rocking it for over a month now, first on an 2013 Dell Optiplex 9090 but it just got an upgrade!

* AMD Ryzen 5 3600
* Gigabyte x570 UD
* Gigabyte GeForce GTX 1650 super
* 16GB DDR4 RAM

It's the first build I've done in a long time, and the first AMD build I've done.  The old Dell had a couple of hard drives I mirrored with ZFS, but this new machine has a slightly more elaborate setup:

* 2x 240GB SSDs - Boot, swap, OS
* 2x 2TB HDDs - home directory
* 1x 2TB HDD - backup

In this post I'm documenting how the disks are configured.

# Solid State Drives

## Partition Layout

```
Number  Start   End     Size    File system     Name                         Flags
 1      1049kB  1075MB  1074MB  fat32           ssd-top-S1CUNYADC10187-boot  boot, esp
 2      1075MB  3222MB  2147MB  linux-swap(v1)  ssd-top-S1CUNYADC10187-swap  swap
 3      3222MB  218GB   215GB                   ssd-top-S1CUNYADC10187-zfs
```

You can see each partition gets a label stating that it's an SSD, where it is in the case, the serial number, and what the partition is for.

## Boot

Currently the boot partition is a vfat file system containing the required bits for systemd-boot.  It's not currently mirrored and this post will get updated when I figure that bit out.

## SWAP

I created a mirror of the two SSDs second partitions using `mdadm`, issued `mkswap` and told the system to start using it.

```
[adref ~]# mdadm --create swap --level=1 --raid-devices=2 --metadata=0.90 /dev/disk/by-partlabel/ssd-top-S1CUNYADC10187-swap /dev/disk/by-partlabel/ssd-bottom-SA400S37-swap
[adref ~]# mkswap /dev/md/swap
mkswap: /dev/md/swap: warning: wiping old swap signature.
Setting up swapspace version 1, size = 2 GiB (2147414016 bytes)
no label, UUID=97e5dbc0-3d26-4ae3-8bac-95561feee566
[adref ~]# swapon /dev/disk/by-uuid/97e5dbc0-3d26-4ae3-8bac-95561feee566

```

I also copied it to `/etc/fstab` - see end.

## Operating System

I really like the ZFS layout that FreeBSD uses.  It was a while ago and I can't recall how Manjaro sets ZFS datasets up, but I didn't like it, so I used the following create my OS disk:

```
zpool create -o ashift=12 -m none zroot mirror /dev/disk/by-partlabel/bottom-Z4S68HRV-zfs /dev/disk/by-partlabel/top-Z4S47V4C-zfs
zpool export zroot
zpool import -R /mnt zroot
zfs create -o mountpoint=none zroot/ROOT
zfs create -o mountpoint=/ zroot/ROOT/default
zfs set compression=on zroot
zfs set atime=off zroot
zfs create -o mountpoint=/tmp -o exec=on -o setuid=off zroot/tmp
zfs create -o mountpoint=/usr -o canmount=off zroot/usr
zfs create -o mountpoint=/home zroot/home
zfs create -o mountpoint=/var -o canmount=off -o xattr=sa zroot/var
zfs create -o atime=on zroot/var/mail
zfs create -o exec=on -o setuid=off zroot/var/tmp
zfs create zroot/var/cache
zfs create zroot/var/cache/pacman
zfs create -o canmount=off -o mountpoint=/var/lib zroot/var/lib
zfs create zroot/var/lib/docker
zfs create zroot/var/lib/libvirt
zfs create -o canmount=off -o mountpoint=/var/lib/systemd zroot/var/lib/systemd
zfs create zroot/var/lib/systemd/coredump
zfs create zroot/var/log
zfs create zroot/var/log/journal
zpool set bootfs=zroot/ROOT/default zroot
```

_Note:_ The above uses the two home directory drives, I've since copied these to the SSDs but I've not changed the disk names in the above to match the new setup.

# Home Directory Drives

## Partition Layout

```
Number  Start   End     Size    File system  Name              Flags
 3      3222MB  2000GB  1997GB               top-Z4S47V4C-zfs
```

Spot the oddity?  This drive starts around 3GB in because I used to have `/boot` and SWAP partitions on here before they migrated to the SSDs.

## ZFS

ZFS is a little complicated because I've removed the OS datasets from this pool and kept my home directory, but itI've done something like this:

```
zpool create -o ashift=12 -m none pwll mirror /dev/disk/by-partlabel/bottom-Z4S68HRV-zfs /dev/disk
/by-partlabel/top-Z4S47V4C-zfs
zfs create pwll/home
zfs set mountpoint=/home pwll/home
zfs create pwll/home/bil
```

I've got more datasets under my home directory that came from another machine.

_Note:_ `pwll` is Welsh for 'pool'.

# Backup Drive

The backup drive used to be my Time Machine drive on my Mac.  It has a basic partition layout:

```
Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2000GB  2000GB
```

It's got a single dataset:

```
zpool create tad /dev/disk/by-id/ata-MB2000GCWLT_PDJ71PUX-part1
```

And I just `zfs send` snapshot to it periodically.  

_Note:_ `tad` is Sindarin Elvish for 'two'.

# Useful bits

## SSD Partitioning

The below was used to create the required partitions on the boot drive.

```
[adref ~]# sgdisk -n1:0:+1G -t1:ef00 /dev/sdc
The operation has completed successfully.
[adref ~]# sgdisk -n2:0:+2G -t2:8200 /dev/sdc
The operation has completed successfully.
[adref ~]# sgdisk -n3:0:+200G -t3:bf00 /dev/sdc
The operation has completed successfully.

[adref ~]# parted /dev/sdc
GNU Parted 3.3
Using /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) name 1 ssd-bottom-SB400S37-boot                                  
(parted) name 2 ssd-bottom-SB400S37-swap
(parted) name 3 ssd-bottom-SB400S37-zfs
(parted) print                                                            
Model: ATA KINGSTON SA400S3 (scsi)
Disk /dev/sdc: 240GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name                      Flags
 1      1049kB  1075MB  1074MB               ssd-bottom-SB400S37-boot  boot, esp
 2      1075MB  3222MB  2147MB               ssd-bottom-SB400S37-swap  swap
 3      3222MB  218GB   215GB                ssd-bottom-SB400S37-zfs

(parted) quit                                                             
[adref ~]#
```

## fstab

```
# /dev/sda1 UUID=B38A-1CFD
/dev/disk/by-partlabel/ssd-top-S1CUNYADC10187-boot	/boot     	vfat      	rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,utf8,errors=remount-ro	0 0

# /dev/md/swap 97e5dbc0-3d26-4ae3-8bac-95561feee566
UUID=97e5dbc0-3d26-4ae3-8bac-95561feee566	none      	swap      	defaults,pri=-2	0 0
```

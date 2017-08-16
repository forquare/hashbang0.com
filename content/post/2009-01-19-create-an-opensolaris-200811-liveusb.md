+++
date = "2009-01-19T00:48:11"
title = "Create an OpenSolaris 2008.11 LiveUSB"
tags = ["IBM","livecd","liveusb","OpenSolaris","ThinkPad","X32"]
categories = ["OpenSolaris"]
+++

T'other day I won an IBM X32 on eBay (more on that tomorrow).  I want to install OpenSolaris on the laptop, but it doesn't have an optical drive...

I set about looking for a solution.  Unfortunatly, Googling around didn't throw up anything to begin with :(  So I thought I'd record my steps as a walkthru.

First of all, I installed the distributon constructor:

`# pkg install SUNWdistro-const`

Next I cd'd to the directory they had been installed to:

`# cd /usr/bin`

And ran the following command:

`./usbgen /var/tmp/osol-0811.iso /var/tmp/os0811.usb /var/tmp
/dev/rlofi/3:   1689000 sectors in 2815 cylinders of 1 tracks, 600 sectors
824.7MB in 176 cyl groups (16 c/g, 4.69MB/g, 2240 i/g)
super-block backups (for fsck -F ufs -o b=#) at:
32, 9632, 19232, 28832, 38432, 48032, 57632, 67232, 76832, 86432,
1593632, 1603232, 1612832, 1622432, 1632032, 1641632, 1651232, 1660832,
1670432, 1680032
Copying ISO contents to USB image...
..................................................
..................................................
..................................................
.............................
1407024 blocks
=== ./usbgen completed at Mon Jan 19 00:25:49 GMT 2009`

Lastly:

`# ./usbcopy /var/tmp/os0811.usb
Found the following USB devices:
0:      /dev/rdsk/c9t0d0p0      15.5 GB USB DISK 2.0     PMAP
Enter the number of your choice: 
WARNING: All data on your USB storage will be lost.
Are you sure you want to install to
USB DISK 2.0 PMAP, 15500 MB at /dev/rdsk/c9t0d0p0 [ w  (y/n) y
Copying and verifying image to USB device
Finished 824 MB in 316 seconds (2.6MB/s)
0 block(s) re-written due to verification failure
Installing grub to USB device /dev/rdsk/c9t0d0s0
Completed copy to USB`

And that's it!  Should be done.  Will report back tomorrow with how the install went.

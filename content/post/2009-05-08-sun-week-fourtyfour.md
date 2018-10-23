+++
date = "2009-05-08T16:30:22"
title = "Sun - Week Forty-four"
tags = ["LVM","SVM","virtualbox","vmware","zfs"]
categories = ["Sun"]
+++

This week is excellent! Monday starts off as a bank holiday, and I have Thursday and Friday off!

Tuesday consisted of me pulling some vmware VM's from a box in the lab. This shouldn't have been difficult, but the box was running Linux, the VM's were on a SCSI array, and the array had been made into one volume using LVM. There is hardly any documentation out there for LVM! Linux people, get your arse's in gear and take a leaf out of the ZFS book for goodness sake! It reminds me a lot of what SVM used to be.
After finally getting the correct command I found I had to activate the volume, which took even more poking around on the net to find out what to do! After that it was just a case of mounting the volume.

Wednesday was more of the same really, after finding what VM's the engineer actually wanted, I set about deleting them from [VMWare][1] and move the directories sideways, tar'ed them up then scp'd them over to a lab box where I'm just running bzip2 on them, 16GB is a hullofalot to transfer, it took half an hour for each of the VM's to transfer!
Hopefully bzip2 will get the size down somewhat and I can transfer them over to the other lab. I'll need to then import them into [VitualBox][2], and I'm hoping it'll be OK...I'll be following this guide here: [clicky][3]

Thursday was spent travelling up to Aber for the May Ball on Friday

Friday was spent meeting up with friends, etc. and going to the May Ball.

The last few posts were generated on Wednesday and may not be updated, this is scheduled to be published on Friday.

  [1]: http://www.vmware.com/
  [2]: http://www.virtualbox.org/
  [3]: http://www.unix-tutorials.com/go.php?id=4011

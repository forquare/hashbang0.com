---
date: 2017-07-27T21:43:35+01:00
title: "freebsd-upgrade In A Boot Environment"
tags: ["FreeBSD","Upgrading","Boot Environments"]
categories:  ["FreeBSD"]
---

Yesterday [FreeBSD 11.1 was released](https://lists.freebsd.org/pipermail/freebsd-announce/2017-July/001798.html).  Once I got into work I started upgrading the VM I use for day to day activities.  After creating a Boot Environment (BE) using [beadm(1)](https://www.freebsd.org/cgi/man.cgi?query=beadm), and running the upgrade and install parts of [freebsd-update(8)](https://www.freebsd.org/cgi/man.cgi?query=freebsd-update), I rebooted into a newly activated BE only to find I had an 11.1 kernel, but a 11.0 userland…

I had no idea what I'd done wrong.  After some questions on the [FreeBSD Forums](https://forums.freebsd.org/threads/61748/#post-356100), I figured it out.  Previously, I had only run the install process once; the install process needs to be done three times:

1. Install the kernel
2. Install userland
3. Cleanup

Usually, one would reboot between these actions.  This is what I had attempted, but when I rebooted into my new BE I got the familiar message:

    No updates are available to install.
    Run '/usr/sbin/freebsd-update fetch' first.

So, what's the correct procedure?  Hunting across the Internet, I found many examples of how people thought it should be done.  Most commonly was:

1. Create a BE
2. Activate the BE
3. Reboot into the BE
4. Fetch the upgrade
5. Install the upgraded kernel
6. Reboot
7. Install upgraded userland
8. Reboot (sometimes this seemed optional)
9. Run install again for cleanup
10. Reboot

Now, that seems like an awful lot of downtime.  Back at Sun, BEs were introduced to me as a convenient roll-back method if things go wrong, and also to reduce downtime caused by upgrade (this was called [Live Upgrade](/2008/11/21/sun-week-twenty/)).  Having all of this downtime did not appeal to me one bit.

So, how can we reduce downtime while using FreeBSD Boot Environments?  Run all three installation tasks one after each other.  Since we are upgrading an essentially dormant system (the BE hasn't been activated and rebooted into yet) we don't need to do the in-between reboots.  Here's my process:

```
bil@fbsd-bil:~ %>su -
Password:

root@fbsd-bil:~ # beadm create 11.1-RELEASE
Created successfully

root@fbsd-bil:~ # beadm mount 11.1-RELEASE /mnt
Mounted successfully on '/mnt'

root@fbsd-bil:~ # freebsd-update upgrade -b /mnt -r 11.1-RELEASE
src component not installed, skipped
Looking up update.FreeBSD.org mirrors... 4 mirrors found.
Fetching metadata signature for 11.0-RELEASE from update6.freebsd.org... done.
Fetching metadata index... done.
Fetching 1 metadata patches. done.
Applying metadata patches... done.
Fetching 1 metadata files... done.
Inspecting system... done.
[-- SNIP! Lots of stuff here we don't care about seeing--]
/boot/kernel/acl_posix1e.ko
/boot/kernel/acpi_asus.ko
To install the downloaded upgrades, run "/usr/sbin/freebsd-update install".

root@fbsd-bil:~ # freebsd-update install -b /mnt
src component not installed, skipped
Installing updates...
Kernel updates have been installed.  Please reboot and run
"/usr/sbin/freebsd-update install" again to finish installing updates.

root@fbsd-bil:~ # freebsd-update install -b /mnt
src component not installed, skipped
Installing updates...
Completing this upgrade requires removing old shared object files.
Please rebuild all installed 3rd party software (e.g., programs
installed from the ports tree) and then run "/usr/sbin/freebsd-update install"
again to finish installing updates.

root@fbsd-bil:~ # freebsd-update install -b /mnt
src component not installed, skipped
Installing updates... done.

root@fbsd-bil:~ # beadm umount 11.1-RELEASE
Unmounted successfully

root@fbsd-bil:~ # beadm activate 11.1-RELEASE
Activated successfully

root@fbsd-bil:~ # reboot

bil@fbsd-bil:~ %>freebsd-version -ku
11.1-RELEASE
11.1-RELEASE
```

Now you can keep the previous BE around until you're happy everything is working and then destroy it.

I've not tried it, but I see no reason why this wouldn't work for updates (e.g. 11.0-RELEASE-p0 to 11.0-RELEASE-p1) too.

Happy upgrading!

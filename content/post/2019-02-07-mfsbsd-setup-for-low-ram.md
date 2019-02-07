+++
date = 2019-02-07T10:31:00
title = "Setting up mfsBSD for receiving ZFS snapshots on systems with low memory"
tags = ["FreeBSD","ZFS","mfsBSD"]
categories =  ["FreeBSD"]
author = "Ben Lavery-Griffiths"
+++

I recently had a need to boot into a fresh server (a VirtualBox VM, actually) with FreeBSD in order to partition the disk and [make it ready to restore another machine onto it](/2019/02/07/restore-freebsd-from-a-zfs-snapshot/).

Of course I turned to [mfsBSD](https://mfsbsd.vx.sk).  I downloaded the ISO, started the `zfs recv`, and found sometime later I found my VM had lost its disk and there were messages that looked like the VM had run out of memory.  
No problem, let's spin our own mfsBSD ISO!

I did the following on my FreeBSD laptop as root.  
Firstly, download a FreeBSD ISO (the CD is fine).  If you are using this to restore a snapshot of another machine **make sure you download the ISO that matches the release the snapshot was taken on!**  
I did not, initially, and came up with some obscure boot messages.

So, let's take a look at the commands I used:

```
root@bil-bsd # cd /var/tmp

# Fetch the FreeBSD ISO
root@bil-bsd # fetch https://download.freebsd.org/ftp/releases/ISO-IMAGES/11.2/FreeBSD-11.2-RELEASE-amd64-disc1.iso

# Mount the ISO
root@bil-bsd # mdconfig -a -t vnode -u 10 -f /var/tmp/FreeBSD-11.2-RELEASE-amd64-disc1.iso
root@bil-bsd # mount_cd9660 /dev/md10 /mnt/

# Clone the mfsbsd repo
root@bil-bsd # git clone https://github.com/mmatuska/mfsbsd.git
root@bil-bsd # cd mfsbsd
```

Now, we want to copy one of the sample files and modify it:

```
root@bil-bsd # cd conf
root@bil-bsd # cp loader.conf.sample loader.conf
root@bil-bsd # cat << EOF >> loader.conf
vm.kmem_size="330M"
vm.kmem_size_max="330M"
vfs.zfs.arc_max="40M"
vfs.zfs.vdev.cache.size="5M"
EOF
```

I found these settings in the [FreeBSD Handbook's ZFS Advanced Topics page](idp62455800) under "Loader Tunables".

Now we can make our ISO:

```
root@bil-bsd # cd ..
root@bil-bsd # make iso BASE=/mnt/usr/freebsd-dist RELEASE=11.2-RELEASE
```

For full build information, check out the [Build page](https://github.com/mmatuska/mfsbsd/blob/master/BUILD.md) in the mfsBSD repo.

I copied this back to my Mac, booted from it and completed the post linked to at the beginning of this one.
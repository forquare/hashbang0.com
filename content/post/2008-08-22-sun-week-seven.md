+++
date = "2008-08-22T14:43:33"
title = "Sun - Week Seven"
tags = ["autofs","nfs","profiles","raid","roles","Solaris","training","zfs","zones"]
categories = ["Sun"]
+++

More training this week.

We started the week looking at how Solaris interfaces with the network, then went on to look at some networking basics related to Solaris.

On Tuesday we looked at what Swap was and why and how we should use it and also the pros and cons of using it. We looked at managing crash and core dump files too. We spent the afternoon looking at NFS and AutoFS, these go hand in hand to make an NFS server and allows users to automatically mount file systems on their client machines.

Wednesday began with us looking at RAID, specifically RAID 0,1 and 5, looking at how they worked and in what scenarios we would use them. We also looked at the Solaris Volume Manager software (formally Solstice DiskSuite prior to Solaris 9) to manage our volumes.
The afternoon saw us studying the topic I was waiting for, the topic which is a buzzword all around Sun and throughout the UNIX communities: ZFS. ZFS is amazing, it really makes you see how simple volume management can be, and how easy it can be to grow, shrink and RAID volumes. It's a truly marvellous topic to study.

Thursday saw us looking at RBAC. RBAC is a technology which allows you to create Profiles, these Profiles have permissions to do a defined amount of things. Profiles can be assigned to Roles, Roles are like users must be switched to using the _su_ command, this allows you to give root privileges of a certain command to a Role, give the Role a password, then give the Role name and password to a specific user. This way, you could have another member of your admin team be responsible for shutting down a machine, but they still don't have full root rights. You can also assign Profiles directly to a user account.
We also looked at syslog, the daemon responsible for logging various events on the system.
After lunch, we looked at naming services such as DNS and and briefly at LDAP, but we concentrated most on NIS. We set up one of our machines to be a NIS Master server, then another to be a Slave Server. We could then assign users to the NIS database, and log in from other clients machines. We looked at taking the Master offline and seeing the impact on the clients as they switch to the Slave.

On Friday, we concentrated solely on Zones. Zones allow you to run an instance of Solaris 8, Solaris 9 or Solaris 10 or even a couple of versions of Red Hat. It runs an instance of the OS, and acts like a separate OS, however, it doesn't run on it's own Kernel, it passes system calls to the main Solaris 10 Kernel to execute.
This is a great method to not only sandbox systems, but to consolidate them too. Think about it; you have a separate server for your accountants, for your web server, and for your NIS server. But during the day, your web server isn't particularly busy, and during the night your NIS server has no activity at all, your accounts server works during the day, and also at night, but doesn't strain too much.
Now, consolidate three boxes to one; less power consumption, less space taken, etc... Each of these servers now has it's own zone, which will take up resources as it needs (load balancing), and will be isolated from the other zones. Each zone is it's own system, with it's own users, software etc. Some patches are centralised from the Global zone, and they apply through all the zones.

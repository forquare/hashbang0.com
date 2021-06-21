---
date: "2010-03-15T19:20:31"
title: "SUNWyp - A.K.A. NIS"
tags: ["2009.06","NIS+","OpenSolaris","SUNWyp"]
categories: ["General"]
---

So, over the last week I have been trying to get NIS to work with OpenSolaris in VirtualBox machines.

I managed to get the master server to work fine by doing the following:

1. Make sure all hosts are in /etc/inet/hosts
2. Install SUNWyp
3. Set the domain name like so: domainname mydomain
4. Create the default domain file to make domain name persistent across reboots: domainname &gt; /etc/defaultdomain
5. touch ethers, bootparams and netgroup in /etc
6. Copy /etc/nsswitch.nis to /etc/nsswitch.conf
7. Run ypinit -m and answer questions
This list should have started all of the necessary NIS services.  Setting up the clients proved a little more difficult.  All of the documentation I found said to do the following:

1. Make sure all hosts are in /etc/inet/hosts
2. Install SUNWyp
3. Set the domain name like so: domainname mydomain
4. Create the default domain file to make domain name persistent across reboots: domainname &gt; /etc/defaultdomain
5. Copy /etc/nsswitch.nis to /etc/nsswitch.conf
6. Run ypinit -c and name the NIS master server
7. Start NIS client service with svcadm enable nis/client

However, this didn't work.  I tried using host names and IP addresses to identify the NIS master when ypinit asked me to specify it, but it wouldn't work.
As a work around I found that if you don't specify a master server at all, ypbind will use the -broadcast option and find the master automatically.  Using the method above and starting ypbind -broadcast myself didn't seem to work.

This all works on OpenSolaris 2009.06

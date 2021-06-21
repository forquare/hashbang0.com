---
date: "2015-08-04T11:07:03"
title: "Grabbing the MAC address on RHEL"
tags: ["ifconfig","RHEL","network","code snippet"]
categories: ["Linux","Bash/Shell"]
---

At work I deploy Red Hat Enterprise Linux VMs, for a variety of reasons, mostly by hand. 
 
One of the steps I loath is setting up the network, it's almost the only thing that truly requires manually tapping each character out. I have, however, learnt this bash one-liner such that I type it out without thinking: 
 
{{% gist forquare 790518779e0577051aaa %}}
 
Simply replace “eth0” with whatever interface you want the MAC address from and redirect it into the relevant ifcfg- file, edit said file with your favourite editor and prepend “HWADDR=” to the line with the MAC address on.

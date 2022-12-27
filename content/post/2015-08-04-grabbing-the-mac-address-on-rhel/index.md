---
date: "2015-08-04T11:07:03"
title: "Grabbing the MAC address on RHEL"
tags: ["ifconfig","rhel","network","code snippet"]
categories: ["computing"]
---

At work I deploy Red Hat Enterprise Linux VMs, for a variety of reasons, mostly by hand. 
<!--more-->
One of the steps I loath is setting up the network, it's almost the only thing that truly requires manually tapping each character out. I have, however, learnt this bash one-liner such that I type it out without thinking: 
 
```
ifconfig -a | grep eth0 | sed 's/.*r \([0-9A-F:]*\).*/\1/'
```
 
Simply replace “eth0” with whatever interface you want the MAC address from and redirect it into the relevant ifcfg- file, edit said file with your favourite editor and prepend “HWADDR=” to the line with the MAC address on.

+++
date = 2018-11-13T14:33:20
title = "Ansible ad-hoc command on multiple hosts"
tags = ["ansible", "ad-hoc", "automation"]
categories =  ["computing", "automation"]
author = "Ben Lavery-Griffiths"
+++

Quite often I want to run an ad-hoc command against a number of hosts, usually this is a subset of an existing group (and often new nodes added to a group).  

Let's say we had an inventory that looks like this:

	[webhosts]
	web1
	web2
	web5
	
	[jenjins]
	j1
	j2
	j3
	mac-j2
	mac-j3
	wj1

We've just added `mac-j2` and `mac-j3`, run some Playbook against them, but realised we wanted to reboot them after the Playbook for some reason.  

Until today, I thought I had two choices:

1. Put these two hosts into a separate, temporary, group.
2. Run the ad-hoc command against each machine individually.
    
I would usually do the first when I had many hosts, amd the second if I had two or three.  

Today I discovered a third choice (certainly works in anisble 2.7, may work in earlier versions):

* Using spaces, list machines within quotation marks.

So, we can do something like this:

    ansible 'mac-j2 mac-j3' -i boxen -m reboot --become

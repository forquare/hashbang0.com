---
date: "2008-07-30T22:32:31"
title: "etc -> private/etc"
tags: ["filesystem","fstab","os x"]
categories: ["computing"]
---

After delving into Solaris10 a couple of weeks ago, I thought I would explore the hidden regions of my Mac...It turns out that it has many of the same things as a typical UNIX/Linux system, but doesn't seem to use them :?
<!--more-->
For example, /etc, is not there! Its a symlink to /private/etc, which is most weird, and even weirder still, I had a look into fstab, but it's not there! Well, it's called fstab.hd, and here's the contents:

> IGNORE THIS FILE.
This file does nothing, contains no useful data, and might go away in
future releases. Do not depend on this file or its contents.
How weird!

I've noticed this about OS X, it still has a lot of UNIX files in, but doesn't tend to use them...

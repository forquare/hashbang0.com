---
date: "2008-05-19T17:35:44"
title: "Installing GCC on OpenSolaris"
tags: ["gcc","OpenSolaris"]
categories: ["OpenSolaris"]
---

[\*NEW\* Click here for command line instructions, plus how to install gmake and subversion!][1]

Are you using the new OpenSolaris, released May 2008?

If you have tried to ./configure any sources, you will find that you can't, because the gcc isn't in your PATH.
To get gcc on your PATH, do the following:

* System &gt; Administration &gt; Package Manager
* Search all packages for gcc
* Install SUNWgcc
* Once installed, open Terminal (right click on desktop)
* Use: export PATH=$PATH:/usr/sfw/bin
* Now everything should be fine!

  [1]: /2009/01/03/installing-gcc-on-opensolaris/

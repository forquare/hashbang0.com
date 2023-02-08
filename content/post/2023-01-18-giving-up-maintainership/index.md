---
date: 2023-01-18T09:42:19
title: "Giving Up Maintainership"
tags: ["freebsd", "ports"]
categories: ["computing"]
---

I've previously posted that I maintained the FreeBSD Port of [hugo](https://gohugo.io/) ([www/gohugo](/2018/12/28/freebsd-ports-unable-to-use-multiple-github-repos-with-the-same-name/)), the awesome static site generator written in Go.  I heard about it on one of the [Jupiter Broadcasting](https://www.jupiterbroadcasting.com/) podcasts in mid-2016 (I don't remember which one, specifically) and by July 2016 I submitted my [first request](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=211127) for a Port to be added to the tree.

[By September](https://web.archive.org/web/20160915001101/http://hashbang0.com/) of the same year this website had been converted from Wordpress to Hugo.  Looking at Git, it looks like I started converting the site in [August 2016](https://github.com/forquare/hashbang0.com/tree/2c0b44c8327c12318d22028d80ff7bcdda25dc42).
<!--more-->
Over the past six and a half years I've bumped the Port for each version the project has pumped out.  There have been several hurdles to jump through - usually because a new version of [Go](https://go.dev/) changed something, like how dependencies should be defined.  
In these cases, it's taken me some time to figure out how to modify the Port to continue working.

I've developed a few [tools](https://github.com/forquare/freebsd-port-helpers) to help with the porting process, many of which were quite bespoke and have stopped working as the Port matured.

In 2021 I was contacted by members of the FreeBSD Documentation Project, stating that they were going to start using Hugo for the FreeBSD Documentation!  
There was some additional patches for me to include into the Port before they were successfully upstreamed, and going forward I would need to ensure that new versions of Hugo didn't break the Docs Project...No pressure!

The latest change to the Port was to use [Go Modules](https://docs.freebsd.org/en/books/porters-handbook/special/#go-ex1), this greatly reduced the amount of work I needed to do inside the Port's Makefile - when I started the Port, FreeBSD didn't allow dependencies to be pulled in during build time, so we needed to define each dependency in the Makefile so that they could be downloaded before building.  

The major changes would sometimes take a week or two for me to work through, and take a fair amount of time outside of my day job to do.  It would take a long time for me mainly because I wasn't working with the Ports framework often enough, so had to relearn various features each time.

With the recent change to Go Modules, I've had a lot of emails saying that various people's builds are breaking.  As mentioned above, I don't do enough with the Ports framework to know all of the ins and outs.  Often I would email back, and receive nothing in response...

Due to a change in family circumstances, and work demanding ever more brain power, I've decided to pass on the maintainer status of Hugo (and any other Ports I maintain).  

Yesterday I sent an email to the FreeBSD Ports mailing list ([first reply here](https://lists.freebsd.org/archives/freebsd-ports/2023-January/003270.html)) asking how to give up the maintainership and also if there were any volunteers.  
Thankfully I received a volunteer very quickly, and a short while ago my [patch](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=269016) was committed to the Ports tree, and now I am no longer the maintainer.

In reality, it had been a long time since I actually built this site on FreeBSD - mostly I use GitHub Actions (and Travis before then) to build and simply push the result to my FreeBSD server.  
I'll miss being a part of this bit of the FreeBSD ecosystem, but I feared I'd probably start missing updates.  It's better to pass the role to someone new that is excited to maintain it.

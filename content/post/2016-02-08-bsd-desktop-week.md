+++
date = "2016-02-08T18:49:32"
title = "BSD Desktop Week"
tags = ["os x","virtualbox","BSDdesktopWeek","tweetbot","reeder"]
categories = ["Open Source","BSD"]
+++

Last week on Twitter I was promoting BSD on the desktop. 
 
{{< tweet 694207654714015745 >}}
 
I got a small flurry of “likes” and “retweets” regarding a number of posts, and one (I think) real person even posted to [\#BSDdesktopWeek][1]! 
 
{{< tweet 694315072886149120 >}}
 
Nobody that I know of took my up on the offer of switching their everyday desktop to BSD for the week, but then I did only start promoting it the Friday before it started... 
 
But why did I want to promote BSD on the desktop?  Firstly, pretty much all the arguments we had a few years ago about why Linux was good for the desktop are pretty much true for BSD.  Secondly, since I started using [FreeBSD][2] on a laptop at home I have realised just how well engineered the system is, how logical everything feels, and how great the community is. 
 
With discovering the second point above, I have begun to switch my Linux servers to FreeBSD.  With the power of Jails and ZFS, I not have one virtual machine running three different services (persistent IRC client, [GitLab][3] server, and [ownCloud][4] server) all segregated from each other and the whole thing is using less than 20GB of storage.  Management is very easy and super configurable. 
 
Since I came to FreeBSD as a server via the desktop, it is somewhat my hope that casual (techie) desktop users who use a BSD everyday on the desktop might choose a BSD for any future server requirements.  Techie users would also bring with them a wealth of knowledge from other systems to improve the BSD in untold ways, and just playing the number game; more users would encourage software to become more portable and BSD friendly. 
 
How was my week using BSD on the desktop?  I must confess I missed OS X, and it didn't help that I ran FreeBSD in [VirtualBox][5] on my Mac.  I missed single click, Magic Mouse support, keyboard shortcuts (e.g. for generating a hyphen instead of a dash, or printing typographic quotation marks), and certain applications that only run on OS X (mainly [Tweetbot][6] and [Reeder][7]). Although some applications, like [1Password][8], would run well in [Wine][9] others did not (like [Dropbox][10])—1Password uses Dropbox to sync files, so bummer! 
But for general desktopy stuff, it worked really nicely.  I happily did email, wrote most of this blog post, watched YouTube, did some perl script editing, some research, etc.  It just worked as a functional desktop. 
 
In the middle of the week, I found a really beautiful email client that is still in development called [N1][11].  I downloaded the source and attempted to compile, but no luck.  Having [opened a ticket][12], the developers are making an attempt to make sure this works on FreeBSD as well as the other supported systems—which I think is awesome of them! 
 
Next year I think I’ll start promoting earlier, and perhaps try to draw in some support from other BSD users.

  [1]: https://twitter.com/hashtag/BSDdesktopWeek?src=hash
  [2]: http://freebsd.org
  [3]: https://about.gitlab.com
  [4]: https://owncloud.com
  [5]: http://virtualbox.org
  [6]: http://tapbots.com/tweetbot/
  [7]: http://reederapp.com/mac/
  [8]: https://agilebits.com/onepassword
  [9]: https://www.winehq.org
  [10]: https://www.dropbox.com
  [11]: https://github.com/nylas/N1
  [12]: https://github.com/nylas/N1/issues/1176

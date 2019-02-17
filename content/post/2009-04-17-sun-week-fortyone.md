---
date: "2009-04-17T12:50:11"
title: "Sun Week Forty-one"
tags: ["15k","Java email","NIS+","Solaris zones","SunBlade 2000","Ultra 80","vi"]
categories: ["Sun","Java"]
---

This week starts on Tuesday, Monday being Easter Monday and a day dedicated to getting over a "chocolate hangover", whatever one of those is...

Tuesday started with me trying to figure out how to send an email using Java. But before I sunk my teeth into it, I realised I had promised to change the hardware of the stable zones box for some engineers.
The stable zones box is just a box which is set up to create and test Solaris zones on, the current machine (A [Sun Blade 2000][1]) was restarting every 24 hours, seemingly this has something to do with Solaris 10 update 5 and above....
So I got an [Ultra 80][2], set up the zones stuff and made it ready to take the place of the current zones box. At 4pm I scheduled some downtime and swapped the boxes, they are heavy things, and powered up the Ultra 80. I had console access, I had changed the details in NIS+, and I just had to change the hostname, which was easy thanks to [this][3] page :D
At 5pm an engineer decided he really want to play with domains on the stable [15K][4] downstairs, the SC's use our production password so I can't give him access. On top of this, the console to the main SC was dead, it wasn't allowing SSH or telnet connections and wouldn't failover. I didn't have time to look at it, so emailed the guy and left it for Wednesday

Wednesday! Feels like a normal Wednesday too, yesterday felt very much like two days...
The morning was spent fixing that 15K, the console wasn't broken, someone had consoled from the SC to an inactive domain, so I had to ~. out of that. After some investigating, it looked like NIS+ wasn't running on the machine, this being Solaris 9 I've got no idea...I'll have to grab David or James at some point...

Gyaaaargh! It's only Thursday :(
The morning went slow...I didn't get any development done because an engineer wanted me to move a storage array and kept complaining that it didn't work. In the end it was his fault for not enabling something or other...
I also got a new email from the guy who I'm doing this programming for.

FRIDAY! Not a moment too soon either!
OK, so I've listened to what Rob said in the comments from [last weeks entry][5] and started to re-plan things, plus sent an email to the guy I was doing the work for. He came back saying that he would prefer to stay as we were, with one stored procedure which gathers booking numbers based on some simple input and then punts the booking number to a Java application. So I've been working at undoing my changes this morning (note to all: don't do something then tell someone your thinking about doing it for fear they'll say no).
I've gone away from university teachings in as much that I'm not bothering with reading in files by setting up a file and setting up a stream buffer, etc, instead I'm using the UNIX cat command, which works quite nicely :D
It's currently lunchtime and I'm looking at publishing the blog now I think, I came back early as my yogurt had separated and was all icky :( My plan for this afternoon is to finish off my email class which will send the emails and perfect the stored procedures.
I'll update this later to say how I got on...

:Q

\*note to self, Wordpress does not except Vim key bindings - FAIL!\*

  [1]: http://sunsolve.sun.com/handbook_pub/Systems/SunBlade2000/images/SunBlade2000.jpg
  [2]: http://sunsolve.sun.com/handbook_pub/Systems/U80/images/U80.gif
  [3]: http://forums.devshed.com/unix-help-35/changing-hostname-and-ip-on-solaris-10t-343047.html
  [4]: http://sunsolve.sun.com/handbook_pub/Systems/SunFire15K_shared/images/SunFire15K.jpg
  [5]: /2009/04/09/sun-week-forty/

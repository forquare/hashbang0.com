---
date: "2009-04-09T18:38:12"
title: "Sun - Week Forty"
tags: ["connect","database","Java","perl","sybase"]
categories: ["industrial year"]
---

This week I'm going to try and finish that email script...A while ago I was assigned the task of writing a module for our booking system.
<!--more-->
**The Problem**

Engineers can book hardware to escalation numbers (customer cases), however, we know that a number of engineers take advantage of this, and book systems for other, more personal, uses under the same escalation numbers.
These bookings don't end unless:
1) The engineer or a lab team member removes the booking, or
2) The customer case is closed.

**The Solution**

Create something that will check the database for bookings which have been active for more than X number of days and bug the engineer via email to see if they still need the system.

**What I'm doing**

So, after achieving this once in Perl (\*spit\*) and it being little good (because we want to trigger it from the database), I gave it another bash:
This time I'm using a stored procedure (from now on referred to as an SP) which will collect a list of bookings. This will then run a Java application, passing in the booking number as an argument. The Java application will then connect to the database again and call another SP which will recover the booking details (I couldn't pass them in as arguments as the method of calling external scripts from the database is limited to 255 characters).

This is basically where I am now, at the end of the week. Next week I'll need to find a way to get the Java application to email the user of the system, and maybe look at a better way of passing in a list of booking numbers, as having loads of these processes running could seriously slow things down...

I will, at some point, blog about connecting to a Sybase database using Java, this should be fairly generic, I just found other documentation for doing this very poor :(

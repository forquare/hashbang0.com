---
date: "2010-05-31T22:26:37"
title: "Dissertation is over..."
tags: ["bash","CS394","Diss","Dissertation","LaTeX","OpenSolaris","VAES","zones"]
categories: ["Uni","Bash/Shell","OpenSolaris"]
---

I meant to blog about this ages ago!  Better late than never I suppose...

Back in April (April 22nd to be precise) I handed in two copies of my 72 page document which outlines everything about my project.  I wanted to blog about my dissertation while I was doing it, but the thought was always pushed back by other thoughts of actually doing the project.

The project investigated how one could use virtualisation techniques of today and use them to virtualise every application on a machine.  Here's a copy of my abstract:
> In computing, occasionally an application can cause a whole system to be brought down. This wastes valuable time for users and has the potential to lose unsaved data. This project sets out to create a concept whereby applications are virtualised for overall system stability and which appears completely transparent to the
end user. The project attempts to create a system based on this concept using OpenSolaris 2009.06 and a number of its technologies, most notably: Zones.

The project discovers that the choice of technologies used are not yet mature enough to implement the concept, though they are very close to maturing.
As the abstract says, I used OpenSolaris Zones to implement the concept.  I created a script which wraps around the OpenSolaris \`pkg\` command which is used to install applications.  My script creates a new Zone and installs the new application into that Zone.  The application is now basically in it's own operating system, to better describe it, it's as if the application has been installed onto a whole new machine.  The application is accessed using SSH and tunnelling X11 over it, this worked quite well after I had set up the infrastructure, but the user had to input their password when they wanted to launch an application.

The project was really fun, I really enjoyed it and learnt so much.  Now it's finished I'm rather lost as to what to do with myself...

If anyone would like to read my Dissertation or try implementing my project (with is licensed under the BSD licence), please don't hesitate to [contact me][1].  At the end of it all, the documentation consisted of 72 pages in PDF form (that was 1086 lines of LaTeX, excluding comments and blank lines), and the project consisted of 1494 lines of Bash script and 1393 comments!

  [1]: mailto:ben.lavery@gmail.com

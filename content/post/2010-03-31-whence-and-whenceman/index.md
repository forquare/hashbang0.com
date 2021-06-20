---
date: "2010-03-31T11:17:52"
title: "Whence and Whenceman"
tags: ["bash","unix","whence","whenceman","which"]
categories: ["Linux","Mac OS","Bash/Shell","OpenSolaris"]
---

On the UNIX command line, the \`which\` command is great, it tells you where a command is in the system.  However, if your system has two versions of the \`ls\` command, it will only tell you which \`ls\` command you are going to use when tap it in and press enter.  To find all copies of any command, we need something [Liam][1] called \`whence\` (I inherited Liam's bashrc file when working at Sun, and this little gem was right inside it).  The .bashrc function for \`whence\` looks something like this: 
 
{{% gist forquare f4385bbb488f63ef7353 %}}
 
It's nice and straightforward, nothing complicated about it at all.  In fact, to let everyone on your system use it, you could just stick "#!/bin/sh" at the top and stick in a file in /usr/bin ! 
 
Today, I wanted to find a man page, the sysidcfg man page to be precise.  Instead of doing the usual trick (\`find / | grep sysidcfg"), I thought I'd modify \`whence\` to look for it for me, and seeing as it's no longer \`whence\`, I called it \`whenceman\`: 
 
{{% gist forquare 4637753f3c3c742aab63 %}}
 
As you can see, it's very similar, and you could do the same thing by putting it in /usr/bin so everyone could use it.

  [1]: http://lamsey.co.uk/journal.htm

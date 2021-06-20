---
date: "2009-07-30T11:08:01"
title: "XP doesn't do relative links"
tags: ["relative links","unix","UNIX is easy","windows","xp"]
categories: ["Rants"]
---

At work I've got a situation where I want to store a load of files in a central location, and then scatter links across the rest of the file system (for reasons best not explained here).  The file system will be picked up and moved at some point, and indeed everybody accesses this file system through different addresses, so a relative link is exactly what I wanted. 
Under UNIX, I'd do something like this: 
	$ pwd
/var/tmp/test
$ ls -l
drwxr-xr-x 1 user1 other 7 29 Jul 08:00 link_test
-rwxr-xr-x 1 user1 other 29 29 Jul 08:00 foo
$ cd link_test
$ pwd
/var/tmp/test/link_test
$ ln -s ../foo bar
$ ls -l
lrwxr-xr-x 1 user1 other 7 29 Jul 08:01 bar -&gt; ../foo 
I know that I wouldn't be able to do this in Windows anyway, adding the fact that the command line here is off bounds to everyone there was no chance.  But Windows users don't touch 'cmd' anymore, right?  That's why Linux is for geeks...So, there must be a way to do this in the GUI.  Give it a go, you'll get an error. 
 
I wrote to the technical team who, after 24 hours, told me that it couldn't be done.  The worlds most common\* Operating System can't perform this simple operation.  They did give me two work arounds: 
> The usual work around for this is to create a shortcut to the command prompt executable cmd.exe with a switch to start a program or open a file, i.e. cmd.exe /start ..foobar.exe 
And: 
> Create a simple VBS script and run this instead of the shortcut. The code would be as shown below, create a text file and rename the extension to .vbs to execute the code, obviously changing the path as needed. 
`dim objShell 
set objShell = CreateObject("Shell.Application") 
objShell.ShellExecute "..foobar.xls", "", "", "open", 1 
set objShell = nothing` 
I was very disappointed ,and now we have to settle with duplicating files to achieve the same thing (the work arounds didn't work on our system). 
 
\*taken from the W3C page, [here][1]

  [1]: http://www.w3schools.com/browsers/browsers_os.asp

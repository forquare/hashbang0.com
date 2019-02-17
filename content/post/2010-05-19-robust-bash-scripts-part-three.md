---
date: "2010-05-19T10:28:00"
title: "Robust Bash Scripts - Part Three"
tags: ["bash script","defensive scripting","making directories","spaces in input"]
categories: ["Bash/Shell"]
---

This is part of a series of posts which should aid anyone writing Bash scripts to make them more robust. 
 
This post offers some advice on how to Bash script defensively. 
 
Be Prepared is the old Scout motto, and never is it more relevant then when scripting in Bash (or any language come to think of it). 
 
**Missing files and directories** 
 
If you are going to be working with files and directories in Bash (and you probably will be 99.9% of the time) you should test to see if they exist. 
 
You can do a very generic test like this: 
 
	MYFILE=/tmp/test
if [[ -e $MYFILE ]]; then
#Do something
else
#Do something else
#Perhaps create file/directory
fi 
 
The "-e" will test for the existence of ANY file, including directories.  You will probably be better off using "-d" for directories though, and "-f" for regular files, see the man page for more information about test (use the command "man test" or "help test"). 
 
If you are creating a directory somewhere like "/opt/local/share/lib/foo/bar", you should either test for the existence of each folder, or you can simple used the "-p" flag on the "mkdir" command, this will create any folders that aren't created. 
 
**Spaces** 
 
Sometimes your script will deal with text that has spaces in it, and you need to be prepared for that, even if you are sure it will never need to handle spaces, there will always be one occasion somewhere down the line. 
 
If we take this example: 
 
	for EACH in $@;
do
echo $EACH
done 
 
Say the input to the script reads something like this: 
bash myscript.sh hashbang0 "Ben Lavery" Aberystwyth 
The for loop shown previously would print out the output like this: 
hashbang0 
Ben 
Lavery 
Aberystwyth 
 
If we quote the $@ like this: 
 
	for EACH in "$@";
do
echo $EACH
done 
 
The output now becomes: 
 
hashbang0 
Ben Lavery 
Aberystwyth 
 
**Failing Gracefully** 
 
If you are updating a lot of files in a directory, what happens if your script fails halfway through?  Half of your files are unmodified, and the other half are modified.  This could be disastrous, especially in something like a directory full of web documents. 
 
One solution is to copy the contents of the folder before you start working on them, do the work, move the copied files over the old files.  Something a bit like this: 
 
	cp -a /tmp/mydir /tmp/mydir-temp
#modify files in /tmp/mydir-temp
mv /tmp/mydir /tmp/mydir-BAK #backup
mv /tmp/mydir-temp /tmp/mydir 
 
As long as you do the relevant testing and exit the script if anything goes wrong, the script will never overwrite /tmp/mydir, and if it does, you have a backup!

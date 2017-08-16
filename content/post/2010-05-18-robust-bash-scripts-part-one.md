+++
date = "2010-05-18T15:10:19"
title = "Robust Bash Scripts - Part One"
tags = ["bash script","nounset","set -u"]
categories = ["Bash/Shell"]
+++

This is part of a series of posts which should aid anyone writing Bash scripts to make them more robust. 
 
This post shows you how you can guard against unset variables. 
 
Bash doesn't provide any sort of checking for unset variables by default.  You may have something similar to the following piece of code: 
	#!/bin/bash
#Script is called delete.sh
DEL=$1
rm -rf ~/$DEL 
You would use it similar to this: \`bash delete.sh tmp\`, which would delete ~/tmp.  Imagine if you forgot to add the "tmp" on the end...POOF!  That's your home directory gone! 
 
You should get into the habit of using the Bash option "nounset", otherwise known as "set -u".  If you put that at the top of your script and run the command without any arguments, you'll get something like this: 
 
	$ bash delete.sh
delete.sh: line 3: $1: unbound variable 
 
This is somewhat similar to a runtime error in Python or Perl and could be very annoying, but at least you've saved your home directory, or another critical location on your computer! 
 
As a side note, if "set -u" or "set -o nounset" is specified at the top of the script and the script exists because of an error, it will exit with an exit status of 1.  In a later post I will explain how to use the \`trap\` statement to catch this error.

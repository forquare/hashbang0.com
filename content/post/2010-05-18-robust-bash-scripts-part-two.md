---
date: "2010-05-18T15:22:26"
title: "Robust Bash Scripts - Part Two"
tags: ["bash script","errexit","set -e"]
categories: ["Bash/Shell"]
---

This is part of a series of posts which should aid anyone writing Bash scripts to make them more robust. 
 
This post shows you how to use Bash's built-in error checking. 
 
Sometimes you might write a script where one line of the script may fail, this failure could cause the rest of your script to fail horribly! 
 
For example, you have two scripts: One which sets up the network on a machine, and another which sets up a naming service based on details given in the network script.  The second script calls the network setup script before it sets up the naming service. 
 
If the network setup fails for some reason, the script shouldn't attempt to set up the naming service as something has gone wrong. 
 
You might try something like this: 
 
	bash setup_network.sh
if [ "$?"-ne 0]; then
echo "command failed"
exit 1
fi
#start setting up naming service 
 
Although this is fine, what happens if you forget to add it?  Or assume that the network will always be set up so decide not to add it?  If the network doesn't get setup, you'll be in a bit of a pickle! 
 
Bash provides its own, built-in, error checking for you in the form of the "errexit" option, or "set -e".  If a command fails in a script with this option enabled, the script will automatically stop with an error code 1. 
 
If you need to turn off error checking (and you may need to if you are using something like grep which may fail, but you know you've handled it), then you could do this: 
 
	command1
command2
set +e
command_that-may-fail
set -e
command4 
 
"set +e" will turn off error checking. 
 
In a later post I will look at the \`trap\` statement to catch the error this option throws.

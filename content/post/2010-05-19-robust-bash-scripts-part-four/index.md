---
date: "2010-05-19T10:37:55"
title: "Robust Bash Scripts - Part Four"
tags: ["bash script","traps"]
categories: ["Bash/Shell"]
---

This is part of a series of posts which should aid anyone writing Bash scripts to make them more robust. 
 
This post describes how you can use traps to help your code fail more gracefully. 
 
Sometimes you write a script, it fails, and leaves a shed load of temporary files over your file system.  Or perhaps (like in part two) some stuff gets set up, your script fails and leaves a network half set up or a naming service half completed.  Whatever the problem, we can trap them (well, almost all of them…). 
 
For my example, I'm going to use a section of my dissertation.  I have a script which is called "INSTALL.sh", this sets some stuff up itself, and also delegates some tasks to other scripts.  Here is the general gist: 
 
Check for root permissions - If no permissions, exit with status 1 
Move some files about 
Create some directories and move files to those directories 
Call a script to set up the network 
Install NIS (A.K.A. YP) 
Set up NIS 
Set up ability to share home directories 
Call a script to set up a Solaris Zone (virtual machine) 
Move a customised file over a system file 
Call a cleanup function 
 
Now, if any part of that script goes wrong, the script will fail (it will because I've used the "set -e" option).  If the script exists half of the configuration is done, and if you try to run the script again, it will fail because some of the setup is already complete. 
 
To get out of this horrible situation, I have used a trap.  If the script exists with an exit status of 1 or greater, or if it is interrupted by a HUP, INT, QUIT or TERM signal, it will be trapped and run a function called "abort".  Here is my trap statement: 
 
trap 'abort' 1 2 3 15 
 
My "about" method looks something like this: 
 
	abort(){
echo "ABORTING!  Please take note of any warnings!"
``#If pkg-manage exists, rename it to pkg
if [[ -a /usr/bin/pkg-manage ]]; then
rm /usr/bin/pkg
mv /usr/bin/pkg-manage /usr/bin/pkg
fi
#Call cleanup to clear away temp files
cleanup
#Remove contents of /var/vaes
rm -rf /var/vaes
#Remove config file
rm $CONFIG_FILE
#Uninstall NIS & related things
pkg uninstall SUNWyp
domainname ""
rm /etc/defaultdomain
cp /etc/nsswitch.files /etc/nsswitch.conf
zfs set sharenfs=off rpool/export/home
cat /etc/auto_home | grep -v "`hostname`:/export/home" > /tmp/auto_home
mv /tmp/auto_home /etc/auto_home
#Delete zones dir
POOL=`zfs list | awk '{ print $1 }' | 
grep "export" | sed 's/([a-zA-Z]*)/.*/1/g' | head -1`
zfs destroy -Rf $POOL/export/vaes-zones
#Undo zones
bash /tmp/zone_setup.sh abort
#Undo networking
bash /tmp/network_setup.sh abort
}`` 
 
By using the trap statement, you can clean up after your failed script in the majority of cases.  A word of warning though: You cannot trap the KILL (or 9) signal, if your script fails because it has received a KILL signal, it will just exit.

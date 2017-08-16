+++
date = "2010-03-26T19:21:37"
title = "Making a script run on startup on OpenSolaris"
tags = ["OpenSolaris","script","SMF","startup"]
categories = ["General"]
+++

While working on my dissertation, I found that virtual NICs and etherstubs don't automatically reappear on a restart.  So, I have a nice little script to make them come back: 
	#!/bin/bash
/sbin/dladm up-aggr
/sbin/dladm up-vlan
/sbin/dladm up-vnic
/sbin/dladm init-linkprop -w 
I need this to run on startup. 
 
Now, as of Solaris 10, things like this should really be done in SMF.  We can easily create a new service using the following template (modify as you require :) ) : 
 
` ` 
	
































MY_SCRIPT








 
Now, you put that into: 
/var/svc/manifest/site 
It should be named something like "my\_script.xml" where "my\_script" is a descriptive name of your script to run. 
On the command line, as root or using \`pfexec\` run the following commands: 
	cd /var/svc/manifest/site
svccfg validate my_script.xml
svccfg import my_script.xml
svcadm enable my_script 
Now if you restart, your script will execute! 
Thanks to the wonderful people at the opensolaris.org discussion boards for their help!

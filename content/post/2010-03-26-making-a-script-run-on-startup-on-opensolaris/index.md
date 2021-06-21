---
date: "2010-03-26T19:21:37"
title: "Making a script run on startup on OpenSolaris"
tags: ["OpenSolaris","script","SMF","startup"]
categories: ["General"]
---

While working on my dissertation, I found that virtual NICs and etherstubs don't automatically reappear on a restart.  So, I have a nice little script to make them come back: 

```
#!/bin/bash
/sbin/dladm up-aggr
/sbin/dladm up-vlan
/sbin/dladm up-vnic
/sbin/dladm init-linkprop -w 
```

I need this to run on startup. 
 
Now, as of Solaris 10, things like this should really be done in SMF.  We can easily create a new service using the following template (modify as you require): 

```
<?xml version="1.0"?>
<!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">
<!--
SCRIPT DESCRIPTION GOES HERE
-->

<!--Script name goes here-->
<service_bundle type='manifest' name='SUNWcsr:my_script'>

<!--Script name goes here too-->
<service
name='site/my_script'
type='service'
version='1'>

<create_default_instance enabled='false' />

<single_instance/>

<!--If your script needs to run after a certain milestone has been met
you can specify that here, otherwise delete this bit.
Replace value for milestone you need to meet-->
<dependency
name='milestone'
grouping='require_all'
restart_on='none'
type='service'>
<service_fmri value='svc:/milestone/network' />
</dependency>

<!--Script to run goes here-->
<exec_method
type='method'
name='start'
exec='/usr/bin/bash /bin/my_script.sh'
timeout_seconds='60' />

<exec_method
type='method'
name='stop'
exec=':kill'
timeout_seconds='60' />

<!--This bit makes it run ONCE and makes sure it is NOT restarted!-->
<property_group name='startd' type='framework'>
<propval name='duration' type='astring' value='transient' />
</property_group>

<template>
<common_name>
<loctext xml:lang='C'>
<!--Script name goes here-->
MY_SCRIPT
</loctext>
</common_name>
<documentation>
<manpage title='' section=''
manpath='' />
</documentation>
</template>
</service>

</service_bundle>
```
 
 
Now, you put that into `/var/svc/manifest/site`
It should be named something like `my_script.xml` where `my_script` is a descriptive name of your script to run. 
On the command line, as root or using `pfexec` run the following commands: 

```
cd /var/svc/manifest/site
svccfg validate my_script.xml
svccfg import my_script.xml
svcadm enable my_script 
```

Now if you restart, your script will execute! 
Thanks to the wonderful people at the opensolaris.org discussion boards for their help!

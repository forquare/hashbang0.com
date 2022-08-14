---
date: "2009-03-01T18:27:36"
title: "DynDNS and my server"
tags: ["DynDNS","inadyn","IP","manifest","SMF","svcadm","svccfg","xml","xmllint"]
categories: ["beleg-iâ","OpenSolaris"]
---

I meant to blog this ages ago, and have only just remembered about it!

I wanted a way for me to access my server when I wasn't on the local network.  A friend pointed me to [DynDNS][1], this will give you a domain name (e.g. myname.something.com) which you can update to point to your IP.  However, you need a method for updating the DNS entry, this is needed because every time I switch off my modem and turn it back on, Tiscali give me a new IP.

So, my OpenSolaris box will need to update DynDNS.
DynDNS have a couple of scripts for doing this, and how to install/run them.  [Clicky][2].  I chose to use inadyn ([here][3]).
Only problem now is, I've got to make it run!  And I really didn't want to run it at boot every time...So it was an excellent excuse to play with [SMF][4]!

I moved the script to /lib/svc/method/inadyn-exe and created a config file in /etc/inadyn.conf
Here is my config file with some of the more important data spoofed:

```
admin@beleg-ia:~$ cat /etc/inadyn.conf
--username user1
--password password
--update_period 6000
--background
--alias myserver.example.com
```

I then created a new manifest for SMF.  The manifest file is written in XML and tells SMF what the service is, what services it depends on, what services depend on it, what to do when enabling it, what to do when disabling it and much more.
My manifest looks like this:

```
    <?xml version=”1.0″?>
    <!DOCTYPE service_bundle SYSTEM “/usr/share/lib/xml/dtd/service_bundle.dtd.1”>
    <!–
    This service will kick off inadyn-exe which will update my IP
    Address should it change.
    Ben Lavery has an account with dyndns.com
    Hostname is:
    myserver.example.com
    –>

    <service_bundle type=’manifest’ name=’SUNWcsr:ip-update’>

    <service
    name=’site/ip-update’
    type=’service’
    version=’1′>

    <create_default_instance enabled=’false’ />

    <–! Only create 1 instance –>
    <single_instance/>

    <!– Depend on the network being up –>
    <dependency
    name=’milestone’
    grouping=’require_all’
    restart_on=’error’
    type=’service’>
    <service_fmri value=’svc:/milestone/network’ />
    </dependency>

    <!– Depend on the config file existing –>
    <dependency
    name=’config_data’
    grouping=’require_all’
    restart_on=’restart’
    type=’path’>
    <service_fmri value=’file://localhost/etc/inadyn.conf’ />
    </dependency>

    <!– execute /lib/svc/method/inadyn-exe when svcadm enable is issued –>
    <exec_method
    type=’method’
    name=’start’
    exec=’/lib/svc/method/inadyn-exe’
    timeout_seconds=’60’ />

    <!– Kill the process when svcadm disable is issued –>
    <exec_method
    type=’method’
    name=’stop’
    exec=’:kill’
    timeout_seconds=’60’ />

    <template>
    <common_name>
    <loctext xml:lang=’C’>
    IP updator
    </loctext>
    </common_name>
    <documentation>
    <manpage title=” section=”
    manpath=” />
    </documentation>
    </template>
    </service>

    </service_bundle>
```

So, after placing this in /var/svc/manifest/site/ and calling it ip-update.xml I ran it through an XML checker:
```
 xmllint ip-update.xml
```

Then I imported it into the SMF repository:
```
# svccfg
svc:> validate ip-update.xml
svc:> import ip-update.xml
```

After this I was able to use `svcadm enable ip-update` to start the service, though it will start up after the network service has started.  If the service goes down SMF will try to restart it for me!  Even after two months it keeps DynDNS updated and I can happily SSH in from outside my home network!

NB: You may need to set up SSH forwarding from your home router.

  [1]: http://dyndns.com/
  [2]: https://www.dyndns.com/support/clients/
  [3]: https://www.dyndns.com/support/clients/unix.html
  [4]: https://en.wikipedia.org/wiki/Service_Management_Facility

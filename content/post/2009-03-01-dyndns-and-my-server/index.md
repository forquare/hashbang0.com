---
date: "2009-03-01T18:27:36"
title: "DynDNS and my server"
tags: ["dyndns","inadyn","ip","manifest","smf","svcadm","svccfg","xml","xmllint"]
categories: ["personal projects"]
---

I meant to blog this ages ago, and have only just remembered about it!

I wanted a way for me to access my server when I wasn't on the local network.  A friend pointed me to [DynDNS][1], this will give you a domain name (e.g. myname.something.com) which you can update to point to your IP.  However, you need a method for updating the DNS entry, this is needed because every time I switch off my modem and turn it back on, Tiscali give me a new IP.
<!--more-->
So, my OpenSolaris box will need to update DynDNS.
DynDNS have a couple of scripts for doing this, and how to install/run them.  [Clicky][2].  I chose to use inadyn ([here][3]).
Only problem now is, I've got to make it run!  And I really didn't want to run it at boot every time...So it was an excellent excuse to play with [SMF][4]!

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

I then created a new manifest for SMF.  The manifest file is written in XML and tells SMF what the service is, what services it depends on, what services depend on it, what to do when enabling it, what to do when disabling it and much more.
My manifest looks like this:

```
    <?xml version=â1.0â³?>
    <!DOCTYPE service_bundle SYSTEM â/usr/share/lib/xml/dtd/service_bundle.dtd.1â>
    <!â
    This service will kick off inadyn-exe which will update my IP
    Address should it change.
    Ben Lavery has an account with dyndns.com
    Hostname is:
    myserver.example.com
    â>

    <service_bundle type=âmanifestâ name=âSUNWcsr:ip-updateâ>

    <service
    name=âsite/ip-updateâ
    type=âserviceâ
    version=â1â²>

    <create_default_instance enabled=âfalseâ />

    <â! Only create 1 instance â>
    <single_instance/>

    <!â Depend on the network being up â>
    <dependency
    name=âmilestoneâ
    grouping=ârequire_allâ
    restart_on=âerrorâ
    type=âserviceâ>
    <service_fmri value=âsvc:/milestone/networkâ />
    </dependency>

    <!â Depend on the config file existing â>
    <dependency
    name=âconfig_dataâ
    grouping=ârequire_allâ
    restart_on=ârestartâ
    type=âpathâ>
    <service_fmri value=âfile://localhost/etc/inadyn.confâ />
    </dependency>

    <!â execute /lib/svc/method/inadyn-exe when svcadm enable is issued â>
    <exec_method
    type=âmethodâ
    name=âstartâ
    exec=â/lib/svc/method/inadyn-exeâ
    timeout_seconds=â60â />

    <!â Kill the process when svcadm disable is issued â>
    <exec_method
    type=âmethodâ
    name=âstopâ
    exec=â:killâ
    timeout_seconds=â60â />

    <template>
    <common_name>
    <loctext xml:lang=âCâ>
    IP updator
    </loctext>
    </common_name>
    <documentation>
    <manpage title=â section=â
    manpath=â />
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

After this I was able to use `svcadm enable ip-update` to start the service, though it will start up after the network service has started.  If the service goes down SMF will try to restart it for me!  Even after two months it keeps DynDNS updated and I can happily SSH in from outside my home network!

NB: You may need to set up SSH forwarding from your home router.

  [1]: http://dyndns.com/
  [2]: https://www.dyndns.com/support/clients/
  [3]: https://www.dyndns.com/support/clients/unix.html
  [4]: https://en.wikipedia.org/wiki/Service_Management_Facility

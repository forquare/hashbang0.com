---
date: "2009-01-04T15:15:42"
title: "Static IP on OpenSolaris"
tags: ["/etc/defaultrouter","/etc/nsswitch.conf","/etc/nsswitch.dns","hostname","IP","nwam","svcadm"]
categories: ["OpenSolaris"]
---

From here: [Clicky][1]

Make sure you have various DNS domains in `/etc/resolv.conf`, then:
```
cp /etc/nsswitch.conf /etc/nsswitch.conf.BAK
cp /etc/nsswitch.dns /etc/nsswitch.conf
svcadm restart dns/client
```

After this, edit the file: `/etc/nwam/llp`

It will look something like this:
```
rge0    dhcp
```

Change it to this:
```
rge0    static    192.168.1.18/24
```

I have rge0, you may have something else, don't change that bit!  The IP address it your choice too, the /24 gives you the netmask 255.255.255.0

Restart nwam:
```
svcadm restart network/physical:nwam
```

Add your new IP and hostname into /etc/hosts following the same format as already exists

```
svcadm enable network/physical:default
```

The add just the IP address of your default router to /etc/defaultrouter

```
svcadm restart network/routing-setup
```

Hope that help some people :)

  [1]: http://malsserver.blogspot.com/2008/08/setting-up-static-network-configuration.html

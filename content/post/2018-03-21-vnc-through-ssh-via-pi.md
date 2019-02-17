---
date: 2018-03-21T19:16:27
title: "VNC over SSH via a Raspberry Pi"
tags: ["vnc", "raspberry pi", "ssh", "tunnel"]
categories:  ["computing", "web"]
---

Often when I'm away from home I leave my iMac on in case I need to grab anything from it remotely, plus it's ready to go when I get home.  Usually I access it using SSH, via a Raspberry Pi:

    +-----------+              +---------------+
    | Internet  +--------------> Raspberry Pi  |
    +-----------+              +--+------------+
                                  |
                                  |
                               +--v----+
                               | iMac  |
                               +-------+

This is fine for SSH, but when I want to use other things (for example, VNC) it can be a challenge.

The following is mainly for me, but hopefully helpful to others:

1. On the Raspberry Pi under `/etc.ssh/sshd_config`, make sure that `GatewayPorts` is set to `yes` (`GatewayPorts yes`); if you needed to change it, then go ahead and restart SSHd (possibly `service sshd restart`).  This will allow remotely forwarded ports to bind to all interfaces.
2. SSH to the iMac (via the Raspberry Pi) and then SSH to the Raspberry Pi from the iMac using the following: `ssh -R 5900:localhost:5900 user@raspi` - this will forward port 5900 (the VNC port), making it available on the Raspberry Pi on the same port.
3. On the system you are currently sitting at, SSH to the Raspberry Pi using the following: `ssh -L 5901:localhost:5900 user@raspi.home` - this will forward port 5900 from the Raspberry Pi to 5901 on your localhost (5901 is chosen so it doesn't clash with a local VNC service)
4. Point your VNC viewer at `localhost:5901` and enjoy!

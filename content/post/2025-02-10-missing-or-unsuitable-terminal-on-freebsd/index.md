---
date: 2025-02-10T12:32:00
title: "Missing or unsuitable terminal on FreeBSD"
tags: ["freebsd", "TERM", "tmux", "ssh", "terminal", "Ghostty"]
categories: ["computing"]
---

I recently switched to using [Ghostty](https://ghostty.org) as my terminal emulator on macOS and Linux.  A bunch of people at work started using it and were saying how good it is.  In my experience it's a little underbaked right now, but it's nice enough - and the thing that is keeping me here most of all is a unified terminal experience across macOS and Linux (previously I was using iTerm2 and Tilix respectively).

However, since switching I've come across a few pain points related to the TERM environment variable:

```missing or unsuitable terminal: xterm-ghostty```

```Error opening terminal: xterm-ghostty.```

```WARNING: terminal is not fully functional```

<!--more-->

[Ghostty have some documentation](https://ghostty.org/docs/help/terminfo) to work around this, and this has mostly worked fine - it mainly revolves around copying Ghostty's terminfo to another machine (in my case FreeBSD) using the `infocmp` command to dump the terminfo, and then `tic` to install the terminfo, usually into `/usr/share/terminfo` (which is the case on FreeBSD, when run as root).

However, there is one case where I had already copied my terminfo to a FreeBSD server, and while SSH was no longer complaining TMUX _was_.

After much searching and dismissing of naïve workarounds, I came across a few posts here and there pointing the blame at `ncurses` and not `tmux`.  It seems that `tmux` depends on `ncurses`, and `ncurses` can be configured to look for terminfo in alternative locations.

Off to [FreshPorts](https://www.freshports.org) to figure out what `ncurses` in doing!  It didn't take long to find that the Port configures this [here](https://cgit.freebsd.org/ports/tree/devel/ncurses/Makefile?id=bf1f4b75e89117abdcbb0d86550fcad56097b6ef#n22):

```
CONFIGURE_ARGS=	 --datadir=${PREFIX}/share \
		         --with-terminfo-dirs="${PREFIX}/share/terminfo:${LOCALBASE}/share/site-terminfo" \
		         ...
```

So on FreeBSD `ncurses` is looking for terminfo in either `/usr/local/share/terminfo` OR `/usr/share/site-terminfo` - both of which are incompatible with what `tic` does by default.

For now my own workaround is to do the following:

```sh
cd /usr/local/share
ln -s ../../share/terminfo .
```

This isn't great as it muddies FreeBSD base configuration with "site specific" configuration, but it works well enough for me right now.

A better solution would probably be to configure `tic` to install the terminfo into `/usr/local/share/terminfo`, but without testing I don't know if the likes of SSH will pick this up.  Possibly a patch to the Port is in order, instead.

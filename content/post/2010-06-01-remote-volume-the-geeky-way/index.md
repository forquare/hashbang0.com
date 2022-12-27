---
date: "2010-06-01T00:25:27"
title: "Remote Volume, the Geeky Way"
tags: ["os x","volume"]
categories: ["computing"]
---

Today I was sitting in the chair watching a film playing on my Mac Pro.  After much getting up and sitting back down to turn the light off, tilt the screen, etc. I found that the sound was too quiet, and I was darned if I was going to get up again! 
I had my MacBook sitting on my lap, and was sure I could change the volume from that. 
<!--more-->
After some searching, I found some magic AppleScript to do the job for me!  After some remembering, I managed to execute that AppleScript on the command line.  Here's what I did: 
 
```
osascript -e "get volume settings"
output volume:0, input volume:50, alert volume:100, output muted:true

osascript -e "set volume output volume 50"
```
 
The first command showed me what the current settings were, and the second command allowed me to change the volume to 50%.

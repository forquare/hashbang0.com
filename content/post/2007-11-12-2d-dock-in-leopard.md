---
date: "2007-11-12T10:34:36"
title: "2D Dock in Leopard"
tags: ["2d","3d","aberystwyth","ben","dock","lavery","leopard","os x"]
categories: ["Mac OS"]
---

Have you upgraded to Leopard and decided you really don't like the new 3D 'shelf'?
Never fear! Apple have given us two options...



[<img src="http://i9.photobucket.com/albums/a55/forquare/blog/sidedock.png" width="31" height="366" />][1]**First option**
Our first option is to open System Preferences, open the Dock preferences then place our Dock on the left or right of the screen:[<img src="http://i9.photobucket.com/albums/a55/forquare/blog/pos_on_screen.png" width="460" height="74" />][2]

**Second option**
We have another option for you if you want your 2D Dock at the bottom of the screen.
We have three steps, first step is to open your Terminal, next type in:
defaults write com.apple.dock no-glass -boolean YES
Next type:
killall Dock
This should look like this in your Terminal screen:[![][3]][4]

Once you have done this you Dock should look like this:
[<img src="http://i9.photobucket.com/albums/a55/forquare/blog/2ddocl.png" width="529" height="41" />][5]
If you want to revert back to the 3D Dock simply type the following into the Terminal:
defaults write com.apple.dock no-glass -boolean NO
killall Dock

There you go, your 2D Dock in Leopard.


  [1]: http://i9.photobucket.com/albums/a55/forquare/blog/sidedock.png
  [2]: http://i9.photobucket.com/albums/a55/forquare/blog/pos_on_screen.png
  [3]: http://i9.photobucket.com/albums/a55/forquare/blog/terminal.png
  [4]: http://i9.photobucket.com/albums/a55/forquare/blog/terminal.png
  [5]: http://i9.photobucket.com/albums/a55/forquare/blog/2ddocl.png

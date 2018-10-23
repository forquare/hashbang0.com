+++
date = "2015-04-21T17:41:57"
title = "3rd Party iCloud Apps?"
tags = []
categories = ["Apple","Musings","Internet"]
+++

At home I use an iMac.  When I'm away from my desk I use an iPhone 6.  At work, I'm forced to deal with Windows (though use Linux/BSD VMs where possible). 
 
I have a lot of software on my Mac, a number of apps are “document based” though manage those document internally.  Some of these apps talk really nicely across many of the platforms I use (e.g. [Evernote][1]), other software works incredibly well within the Apple ecosystem (e.g. [OmniFocus][2]).   
Lots of the software I use is “document based” in the real sense of the term, you click “save” and it spurts out a document that resides on your file system and these can be accessed on multiple platforms with various syncing services (e.g. [ownCloud][3], or [Dropbox][4]). 
Then there is iWork.  The trio of apps can save a file to your desktop, or shove it in iCloud.  It works really well on OS X, and on iOS, and a host of other platforms thanks to [iCloud.com][5]. 
 
In 2013, Apple release iWork for iCloud beta.  Users with supported browsers (officially Safari 6.0.3+, IE 9.0.8+, Chrome 27.0.1+) on (I presume) any platform can get access to _any_ of their iWork documents which live in iCloud.  How cool is that!?  So now I can have a look over my budget on my work computer while trying to sort out car insurance in my lunch hour, or give a presentation put together on my Mac on someone else Linux workstation. 
 
At WWDC 2014, Apple announced CloudKit.  CloudKit gives developers some server side infrastructure so that they can think about programming the application, and not get caught up thinking about server logic.  CloudKit provides: 

* iCloud Authentication
* Asset Storage
* Database Storage (both general/public, and per user/private)
* Search
* Notifications 
So, Apple already has apps (albeit in beta) on [iCloud.com][6] which make some of their core apps cross-platform.  So too have they started developers thinking about the cloud. 
Apple could provide a means that Interface Builder compiles to JavaScript and CSS.  Could Swift compile into JavaScript, or be used as-is for limited “heavy lifting”?  With CloudKit, CoreData-type and other facilities are already there, waiting to go. 
Could this give developers a means to deploy their apps onto [iCloud.com][7] and get them out to users on platforms other than OS X and iOS?  Users, by the way, that likely already have an Apple ID and given up their payment methods thanks to over a decade of iDevices and iTunes.

  [1]: https://evernote.com
  [2]: https://www.omnigroup.com/omnifocus/
  [3]: https://owncloud.org
  [4]: https://www.dropbox.com/
  [5]: http://iCloud.com
  [6]: http://icloud.com
  [7]: http://iCloud.com

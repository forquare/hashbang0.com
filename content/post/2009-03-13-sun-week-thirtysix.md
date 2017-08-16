+++
date = "2009-03-13T19:41:50"
title = "Sun - Week Thirtysix"
tags = ["J4200","ldom","SunBlade 2000","virtual lab","virtualbox"]
categories = ["Sun"]
+++

Monday started rather differently to usual. we had our lab staff meeting at 10am so we could include the APAC lab manager on the call, he had some interesting things to tell us about collecting statistics about how the lab was used, and how we are going to implant some more measures in to get more accurate results.
We still had the meeting in the afternoon to talk about our own lab, after this meeting we went down and did a lab tidy in preparation for the AMER lab managers visit on Wednesday. I also helped Paul rack some new machines which will be used for bookable [LDOM's][1], as well as a virtual lab host. The virtual lab host has a combination of zones and [VirtualBox][2] I think, these can be provisioned as and when needed.

Tuesday configured a multi-head Sun Ray config for an engineer. I also looked at why the JBOD on the virtual lab box seemed to be winging. It would appear that the [J4200][3] will beep at you if it has an error, and the error could be that some ports aren't plugged into anything! Anyway, the silencer button was pressed and it seems quite happy...However the x4600 it's attached to is complaining of a failed PSU (though no PSU claim to have failed) and a failed fan, thus causing the "system to hot" light to illuminate.

Wednesday we had the pleasure of meeting the AMER lab manager. He had come to see Paul and also interview someone for their industrial year 09-10.
On another note I had a SunBlade 2000 which wasn't pingable, and wouldn't ping anything else...With Ali's help I discovered that it was on the wrong network...It needed to be on the 129.156.175.X network, but it was on the 129.156.208.X network. I configured the VLAN and it all worked. After lunch some other machines were having problems, and it would appear I had knocked them onto the wrong network! I revered my changes and went to plug the SunBlade 2000 into a different switch.

Thursday and Friday was spent doing some minor things such as removing cables from machines, adding PCI cards and jumpstarting machines.

  [1]: http://www.sun.com/servers/coolthreads/ldoms/index.jsp
  [2]: http://www.virtualbox.org/
  [3]: http://www.sun.com/storage/disk_systems/expansion/4200/

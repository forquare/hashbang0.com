---
date: "2008-01-28T21:47:57"
title: "Coding week - Day 1"
tags: ["coding week","encoding","iso-8859-1","tv-anytime","xml","xpath"]
categories: ["uni"]
---

Today started off with rolling out of bed at 8:10am, out the house for 8:50, I had left it a tad late to make it up the hill in time, though my phone helped, playing a crap, compressed version of [99 Luftballons][1].
<!--more-->
I got into the computer room to find some of the group huddled around our computers. Me a Steve got stuck in with looking at how to parse XML using Java and query it using XPath. By 10am we had cracked the parsing; the files we had been given declared they were UTF-8 encoded, however, every time we tried to parse the file we got an error stating something about 1 of 1-byte not UTF-8 or some such nonsense...After looking around, we changed the declaration to ISO-8859-1, this seems to work :D Our data was parsed, or at least displayed no errors!
We battled on with trying to look at XPath, searching various sites, looking at tutorials, but it was useless. We even tried the [TV-Anytime API][2], but our files wouldn't work correctly, the Genre's weren't being recognised...After talking to one of our Project Managers, [Rhys Parry][3], we retreated down into town for lunch.

After lunch we received an email stating that [Nigel Hardy (a.k.a. Beardy Nigel)][4] was putting on a Q&amp;A on XML. Steve and myself attending this meeting and asked about XML namespaces and asked questions relating to out XPath problems.
We returned to work feeling very informed, but not overly sure on where to go next. I had already contacted [Andrew McParland][5] from the [BBC][6] who had worked on the TV-Anytime project, with a stroke of luck he replied just after lunch! He suggested editing the API to recognise the new Genres. The class that needed editing was created in 2002 according to the declaration at the top, the files we were using were time-stamped 2007...

So, two plans formed, look more into XPath, or edit the TVAT API...We chose to persist with XPath. At 4:45 we had got nowhere :( We decided to go for changing the API. First of all I tried to parse the XML file with the TVAT API, receiving close to 5000 lines of errors about genres...I copy and pasted around 1500 lines into a text editor (it appeared that the list was repeated several times) and reduced these down to 24 genres in the space of about 30 minutes...After changing the API, compiling and running, we were not just receiving errors concerning that genres weren't recognised, but also some of them weren't formatted correctly! After looking at an example, I noticed that it was not only formatted the same as all the others, it was the first in the list of genres in the edited class! This at 6pm this evening, after this day...Almost nine straight hours of work, we decided to retire for the day and come back to it in the morning...

So, tomorrow is another day...We are open to all suggestions :D Comments welcome :P
But for now...BED!

  [1]: http://youtube.com/watch?v=jQYQTFudrqc
  [2]: http://www.tv-anytime.org/
  [3]: http://www.aber.ac.uk/compsci/public/department/staff-lists/staff-member-profile.php?staff_id=rrp
  [4]: http://www.aber.ac.uk/compsci/public/department/staff-lists/staff-member-profile.php?staff_id=nwh
  [5]: http://www.linkedin.com/in/andrewmcparland
  [6]: http://www.bbc.co.uk

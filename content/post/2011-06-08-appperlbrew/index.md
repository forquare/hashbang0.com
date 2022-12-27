---
date: "2011-06-08T08:00:00"
title: "App::perlbrew"
tags: ["perl"]
categories: ["computing"]
---

Last summer I decided I would learn Perl.  I had put it off for a long time, but after trying and failing at learning Python, I decided to give Perl a go. 
 <!--more-->
Managing the perl installations has been tedious.  I tend to use a lot of the features in v5.10, but MacPorts keeps installing older and newer versions and making the perl command point to v5.08 for some reason.  But today I found App::perlbrew. 
 
[App::perlbrew][1] makes it easy to install Perl into your home directory, or anywhere else for that matter.  If you install perl into your home directory, you can install whatever modules you like, without needing root access/applicable permissions. 
 
On my Mac, all I had to do was issue four commands _et voila_!  I had the latest and greatest version of Perl: 
 
```
curl -L http://xrl.us/perlbrewinstall | bash
echo "source ~/perl5/perlbrew/etc/bashrc" >> .bashrc
perlbrew install perl-5.22.0
perlbrew switch perl-5.22.0
```
 
Now we can see that the new perl is installed and is running from our home directory: 
	benlavery@Talantinc:~ $&gt;perl -v | grep version
This is perl 5, version 22, subversion 0 (v5.22.0) built for darwin-2level

```
benlavery@Talantinc:~ $&gt;which perl
/Users/benlavery/perl5/perlbrew/perls/perl-5.22.0/bin/perl 
```

How very convenient!

  [1]: http://search.cpan.org/dist/App-perlbrew/

+++
date = 2018-12-28T16:07:29
title = "FreeBSD Ports unable to use multiple Github repos with the same name"
tags = ["FreeBSD", "Ports"]
categories =  ["computing", "build"]
author = "Ben Lavery-Griffiths"
+++

I maintain the [hugo](https://gohugo.io) ([www/gohugo](https://www.freshports.org/www/gohugo)) FreeBSD Port.  In a recent release ([0.51](https://github.com/gohugoio/hugo/releases/tag/v0.51)) a situation arose where the dependency “mitchellh/mapstructure” required a fix.  The Github repository for the dependency was forked to “bep/mapstructure” and a [Pull Request to the original repository made](https://github.com/mitchellh/mapstructure/pull/123).  The Pull Request wasn’t pulled into the original repo in time for the 0.51 release (and still haven’t at time of writing), so the developers have instead specified both the original and forked repos as dependencies.

When I got a notification that hugo had been updated, I thought to myself “ooo, new stuff!”, and I set about updating the [0.50 Makefile](https://svnweb.freebsd.org/ports/head/www/gohugo/Makefile?revision=483461&view=markup).

Sometimes the changes are very simple, all I have to do is modify the `DISTVERSION` and `COMMIT_ID`, and test the build.  Other times, dependencies change and I must redefine `GH_TUPLE` with those changes - thankfully for this I have [a small hacky shell script](https://github.com/forquare/freebsd-port-helpers/blob/master/scripts/parse_gomod.sh) to make things easier.  
The most challenging changes I’ve come across yet are where the project has re-imagined how it defines dependencies (hugo has gone from a vendored JSON file, to godep, to gomod).

This release required the usual changes, plus a change to dependencies, including adding this new forked repo.  
At first I didn’t notice it.  Perhaps I should be more vigilant, but if everything works I am generally happy.  My script parsed the `go.mod` file, gave me a list to put into the `GH_TUPLE` variable; I did a `make makesum` with no problems, then I tried to do a `make` and got the following output:

```
===>  Building for gohugo-0.51
hugolib/page_ref.go:21:2: cannot find package "github.com/bep/mapstructure" in any of:
	/usr/local/go/src/github.com/bep/mapstructure (from $GOROOT)
	/usr/ports/www/gohugo/work/hugo-0.51/src/github.com/bep/mapstructure (from $GOPATH)
minifiers/minifiers.go:27:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2 expects import "github.com/tdewolff/minify"
minifiers/minifiers.go:28:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/css expects import "github.com/tdewolff/minify/css"
minifiers/minifiers.go:29:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/html expects import "github.com/tdewolff/minify/html"
minifiers/minifiers.go:30:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/js expects import "github.com/tdewolff/minify/js"
minifiers/minifiers.go:31:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/json expects import "github.com/tdewolff/minify/json"
minifiers/minifiers.go:32:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/svg expects import "github.com/tdewolff/minify/svg"
minifiers/minifiers.go:33:2: code in directory /usr/ports/www/gohugo/work/hugo-0.51/src/github.com/tdewolff/minify/v2/xml expects import "github.com/tdewolff/minify/xml"
*** Error code 1

Stop.
make[1]: stopped in /usr/ports/www/gohugo
*** Error code 1

Stop.
make: stopped in /usr/ports/www/gohugo
```

Hmm, odd!  It seems that “bep/mapstructure” is missing…Let’s have a look in the Makefile to double check…

```
# grep -n mapstructure Makefile
31:		bep:mapstructure:bb74f1d:mapstructure/src/github.com/bep/mapstructure \
63:		mitchellh:mapstructure:v1.0.0:mapstructure/src/github.com/mitchellh/mapstructure \
```

It is there, as is “mitchellh/mapstructure”.  Very odd!  Let’s have a look in distinfo:

```
# grep -n mapstructure distinfo 
88:SHA256 (gohugo/mitchellh-mapstructure-v1.0.0_GH0.tar.gz) = 6eddc2ee4c69177e6b3a47e134277663f7e70b1f23b6f05908503db9d5ad5457
89:SIZE (gohugo/mitchellh-mapstructure-v1.0.0_GH0.tar.gz) = 18841
```

OK, so only “mitchellh/mapstructure” made it into distinfo...Just for fun, let’s put “bep/mapstructure” below “mitchellh/mapstructure” in the Makefile and see what happens:

```
# grep -n mapstructure Makefile
62:		mitchellh:mapstructure:v1.0.0:mapstructure/src/github.com/mitchellh/mapstructure \
63:		bep:mapstructure:bb74f1d:mapstructure/src/github.com/bep/mapstructure \

# make makesum

# grep -n mapstructure distinfo
88:SHA256 (gohugo/bep-mapstructure-bb74f1d_GH0.tar.gz) = 5bf27fc22a2feb060c65ff643880a8ac180fac9326a86b82d6a3eabe78fa9738
89:SIZE (gohugo/bep-mapstructure-bb74f1d_GH0.tar.gz) = 18666
```

Ah ha!  Right, something is causing later declarations of the same repo name to overwrite earlier ones.   
At this point I took to #freebsd-ports on Freenode and asked some questions.  After a few comments about the reason this was needed, and a reminder that Go based Ports must be [fully-contained](https://www.freebsd.org/doc/en/books/porters-handbook/go-libs.html), vishwin suggested I take a look at `bsd.sites.mk` at around line 371 - this is where `USE_GITHUB` is defined.

Opening up `bsd.sites.mk`, I realised that my Makefile skills were severely lacking!  However, looking above line 371 I noted this:

```
    352 # In order to use GitHub your port must define USE_GITHUB and the following
    353 # variables:
    354 #
    355 # GH_ACCOUNT    - account name of the GitHub user hosting the project
    356 #                 default: ${PORTNAME}
    357 #
    358 # GH_PROJECT    - name of the project on GitHub
    359 #                 default: ${PORTNAME}
    360 #
    361 # GH_TAGNAME    - name of the tag to download (2.0.1, hash, ...)
    362 #                 Using the name of a branch here is incorrect. It is
    363 #                 possible to do GH_TAGNAME= GIT_HASH to do a snapshot.
    364 #                 default: ${DISTVERSION}
    365 #
    366 # GH_SUBDIR     - directory relative to WRKSRC where to move this distfile's
    367 #                 content after extracting.
    368 #
    369 # GH_TUPLE      - above shortened to account:project:tagname[:group][/subdir]
    370 #
    371 .if defined(USE_GITHUB)
```

The definition of `GH_TUPLE` is interesting, having looked at other Ports Makefiles for examples, I had always seen the `[:group]` part to be a repeat of `project`, but it is not.

Looking in the [FreeBSD Porters Handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook), specifically at [Fetching Multiple Files from GitHub](https://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook/makefile-distfiles.html#makefile-master_sites-github-multiple), Example 5.15 shows the following:

```
PORTNAME=	foo
DISTVERSION=	1.0.2

USE_GITHUB=	yes
GH_ACCOUNT=	bar:icons,contrib
GH_PROJECT=	foo-icons:icons foo-contrib:contrib
GH_TAGNAME=	1.0:icons fa579bc:contrib
GH_SUBDIR=	ext/icons:icons

CONFIGURE_ARGS=	--with-contrib=${WRKSRC_contrib}
```
> This will fetch three distribution files from github. The default one comes from foo/foo and is version 1.0.2. The second one, with the icons group, comes from bar/foo-icons and is in version 1.0. The third one comes from bar/foo-contrib and uses the Git commit fa579bc. The distribution files are named foo-foo-1.0.2_GH0.tar.gz, bar-foo-icons-1.0_GH0.tar.gz, and bar-foo-contrib-fa579bc_GH0.tar.gz.

What the above is doing is defining *groups* (“icons” and “contrib”) on the `GH_ACCOUNT` line, then using these groups on subsequent lines to collect details for each project.  Effectively, it says:

1. Download the project **foo** from account **foo** that is tagged **1.0.2**
2. Download project **foo-icons** from project **bar** that is tagged **1.0** and move extracted files to **ext/icons**
3. Download project **foo-contrib** from account **bar** that is commit **fa579bc**

Although in my opinion this usage is a mess, it is obvious that by using these *groups* we can group together multiple github entries.  

Now moving onto Example 5.16 which shows the usage of `GH_TUPLE`:

```
PORTNAME=	foo
DISTVERSION=	1.0.2

USE_GITHUB=	yes
GH_TUPLE=	bar:foo-icons:1.0:icons/ext/icons \
		bar:foo-contrib:fa579bc:contrib

CONFIGURE_ARGS=	--with-contrib=${WRKSRC_contrib}
```
> Grouping was used in the previous example with bar:icons,contrib. Some redundant information is present with GH_TUPLE because grouping is not possible.

When I first read the documentation, I struggled with that last bit.  I took it to mean that no grouping was done.  However, I started to wonder whether it *did* matter.  I changed the *group* from **project** to **account_project**:

```
# grep -n mapstructure Makefile
31:		bep:mapstructure:bb74f1d:bep_mapstructure/src/github.com/bep/mapstructure \
63:		mitchellh:mapstructure:v1.0.0:mitchellh_mapstructure/src/github.com/mitchellh/mapstructure \
```

After doing another `make`, it worked!  Grouping was being done on `GH_TUPLE` entries, and I don’t think it’s meant to!  I have filed [Bug 234468](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=234468) describing the issue, in part to open discussion, and in part to either clarify the documentation or get `Mk/bsd_sites.mk` changed to ignore groups in `GH_TUPLE` entries.

I’ve updated my hacky helper scripts to take this into account, and I’ve updated the Port successfully twice now.  
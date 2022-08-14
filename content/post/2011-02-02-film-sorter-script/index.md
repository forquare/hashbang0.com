---
date: "2011-02-02T00:47:31"
title: "Film Sorter Script"
tags: ["imdb","imdb::film","Internet","media","perl","server"]
categories: ["Perl"]
---

On my server I have a store of films located at /share/media/movies.  Using NFS I share this directory to various other computers on my home network.  For a long while Faye has been asking me to categorise them into genres and cast, but with over 300 files I've been loath to do it by hand, so I thought I'd automate things using Perl.

Using the IMDB::Film module I have been able to automate all of the data gathering.  The module goes off and searches [IMDB][1] for a matching title (in this case I try searching for the file name sans the extension.  Once found you can query the returned object about data such as cast member, genre, director, etc.
Using this data I then create new folders based on the genre name or individual cast names, then symlink the original file to the new folder.  When I mount the general movie folder and the sorted movie folder, everything ties in well and works pretty nicely!

It does have it's flaws, a couple of films have come back and been the wrong one as IMDB confused the title, but overall it does the job.

If you know a bit of Perl, here is the script for you to modify for your own needs:

```
#!/usr/bin/perl

=head1 License
Copyright 2011 Ben Lavery. All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are
permitted provided that the following conditions are met:

   1. Redistributions of source code must retain the above copyright notice, this list of
      conditions and the following disclaimer.

   2. Redistributions in binary form must reproduce the above copyright notice, this list
      of conditions and the following disclaimer in the documentation and/or other materials
      provided with the distribution.

THIS SOFTWARE IS PROVIDED BY Ben Lavery ``AS IS'' AND ANY EXPRESS OR IMPLIED
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL Ben Lavery OR
CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

The views and conclusions contained in the software and documentation are those of the
authors and should not be interpreted as representing official policies, either expressed
or implied, of Ben Lavery.
=cut

use 5.010;
use File::Spec;
use IMDB::Film;
use Cwd;

# Get current working directory
my $cwd = cwd();

# Set up log file
$LOGFILE='/var/tmp/media_script_log';
open(LOGFILE, "&gt;&gt;", "$LOGFILE") or die "can't open $LOGFILE: $!";

# Set up where the categories are to be stored
$SORTED = "/share/media/sorted";
$GENRE = "$SORTED/genre/";
$ACTOR = "$SORTED/actor/";

# The directory that contains the media is the first argument
$DIRECTORY = $ARGV[0];
die "Directory doesn't exist. Stopped" unless -d $DIRECTORY;

# Get list of films
opendir(DIRHANDLE,"$DIRECTORY") or die "Cannot open $DIRECTORY.  Stopped";
my @files = sort readdir(DIRHANDLE);
close(DIRHANDLE);

# Iterate through the list of films and act on each
foreach $media (@files){
	chdir($DIRECTORY);

	# Skip folders and hidden Mac OS X files
	next if ($media eq ".DS_Store");
	next if (-d $media);

	say "Starting: $media";

	# Set up variables
	my $title = undef;
	my @starring = undef;
	my @genre = undef;
	my $IMDB = undef;

	# Strip extention
	$_ = "$media";
	s/(.*)..*/$1/;

	my $error_incurred = 1;
	while($error_incurred){
		$@ = undef;
		# Create IMDB object and search for the title
		eval{$IMDB = new IMDB::Film(crit =&gt; $_) || say "UNKNOWN ERROR - RETRYING";};
		#Check for errors.  If there are errors, try again
		$error_incurred = 0 unless $@ &amp;&amp; (!$IMDB-&gt;status);
	}

	# Find the correct title
	# This is because "The Lord Of The Rings 2" gets translated to "The Lord Of The Rings: The Two Towers"
	$title = $IMDB-&gt;title();
	say "tTitle is: $title";

	$error_incurred = 1;
	while($error_incurred){
		$@ = undef;
		# Search for actual title
		eval{$IMDB = new IMDB::Film(crit =&gt; $title) || say "UNKNOWN ERROR - RETRYING";};
		#Check for errors.  If there are errors, try again
		$error_incurred = 0 unless $@ &amp;&amp; (!$IMDB-&gt;status);
	}

	# Get lists of cast and genres
	@starring = @{$IMDB-&gt;cast()};
	@genre = @{$IMDB-&gt;genres()};

	# Add films to sorted directory
	chdir $GENRE;
	foreach $genre (@genre){
		# If genre directory doesn't exist, create it
		mkdir $genre unless (-d $genre);

		# Calculate relative path between media directory and sorted directory
		my $genre_specific_dir = "$GENRE/$genre";
		my $path = File::Spec-&gt;abs2rel($DIRECTORY, $genre_specific_dir);
		$path .= "/$media";

		my $newpath = "$GENRE/$genre/$title";

		# Create symlink
		symlink($path, $newpath);
	}

	chdir $ACTOR;
	foreach $actor (@starring){
		# Get name of the current actor
		my $name = $actor-&gt;{name};

		# If genre directory doesn't exist, create it
		mkdir $name unless (-d $name);

		# Calculate relative path between media directory and sorted directory
		my $actor_specific_dir = "$ACTOR/$name";
		my $path = File::Spec-&gt;abs2rel($DIRECTORY, $actor_specific_dir);
		$path .= "/$media";

		my $newpath = "$ACTOR/$name/$title";

		# Create symlink
		symlink($path, $newpath);
	}

	# Print message saying that the current media file has been processed
	say "FINSHED.";

}

close(LOGFILE);

chdir $cwd;
print "a";
```

  [1]: http://www.imdb.com/

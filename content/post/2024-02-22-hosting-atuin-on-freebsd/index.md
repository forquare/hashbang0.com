---
date: 2024-02-22T21:22:00
title: "Hosting Atuin on FreeBSD"
tags: ["freebsd", "shell history", "atuin"]
categories: ["computing"]
---

In the past year or so my podcast listening habits have changed.  While I used to favour shows from the [Jupiter Broadcasting network](https://www.jupiterbroadcasting.com/), I have recently found myself listening more and more to podcasts from the [Late Night Linux family](https://latenightlinux.com/about/).

On a recent episode of [Linux Matters](https://linuxmatters.sh/10/) [Martin Wimpress](https://wimpysworld.com/) mentioned that he had started to use [Atuin](https://atuin.sh/) to sync his shell history across multiple machines and gain better history management and insights.

Atuin works by hooking into your shell and storing the history in its own, local, SQLite database.  Optionally you can sync your history to the official Atuin server and then have it available across multiple machines.  Although the history is end-to-end encrypted, Atuin can also be self-hosted - so of course I couldn't help myself from doing just that!

<!--more-->

# Installing Atuin

Installing Atuin on FreeBSD is incredibly easy since there is already [a Port for it](https://www.freshports.org/shells/atuin/), all that is required is `pkg install atuin`.  This single binary is capable of being both the client and the server.

However, let's back up a little - since I am installing this as a service, I first needed to set up a jail for it:

```shell
iocage create -T -r 14.0-RELEASE -n atuin
iocage set boot=on atuin
iocage set ip4_addr="lo1|172.16.0.10" atuin
```

With the jail created, I logged into it and installed Atuin.  I noted that the port doesn't install an `rc.d` script for running the server, so I whipped a quick one up:

```shell
#!/bin/sh

# PROVIDE: atuin
# REQUIRE: LOGIN DAEMON NETWORKING
# KEYWORD: rust

# Enable this script by adding:
# atuin_enable="YES"

. /etc/rc.subr
name=atuin

rcvar=atuin_enable
load_rc_config ${name}

atuin_chdir=/home/atuin
atuin_user=atuin
atuin_group=atuin
atuin_env="HOME=${atuin_chdir}"

# This is the tool init launches
command="/usr/sbin/daemon"

pidfile="/var/run/${name}/${name}.pid"

task="/usr/local/bin/${name} server start"
procname="/usr/local/bin/${name}"

command_args="-p ${pidfile} -T ${name} ${task}"

start_precmd="start_precmd"
start_precmd()
{
        if [ ! -e "/var/run/${name}" ] ; then
                install -d -o ${atuin_user} -g ${atuin_group} /var/run/${name};
        fi
}

run_rc_command "$1"
```

I created an `atuin` user and group for the service to run as, and then configured the service - unfortunately the configuration is required to live under the users home directory: `~/.config/atuin/config.toml`. My configuration is mostly the default, but where it differs is:

```toml
# Listen on all interfaces (only l01 available in jail)
host = "172.16.0.10"
# Enable registration
open_registration = true
# Database connection
db_uri="postgres://atuin:superstrongpassword@172.16.0.11/atuin"
```

This is also where I found that I needed to set up a PostgreSQL database for Atuin to use.

# Configuring PostgreSQL

I haven't used PostgreSQL much in the past, so the following is more of a note to myself than anything else.

I created a new jail, installed PostgreSQL and created a database and user for Atuin to use:

```postgresql
create user atuin with password 'superstrongpassword';
create database atuin;
grant all privileges on database atuin to atuin;
alter database atuin owner to atuin;
```

I also had to edit `~postgres/data16/pg_hba.conf` to allow the `atuin` user to connect to the database:

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
host    atuin           atuin           172.16.0.10/32          trust
```

I went back to the Atuin jail and started the service.

# Reverse Proxy

Very broadly, my setup looks like this:
* Traffic comes into my server either via a public IP or via a VPN
* The PF firewall is configured to redirect traffic to the appropriate jail (e.g. traffic on ports 80 and 443 is redirected to the web jail)
* Nginx is configured to proxy traffic to the appropriate service dependent on the hostname.  Nginx can also restrict access to services to only allow access from the VPN
* Furthermore, certain domains are configured in DNS with the internal VPN address, so won't be routable from the public internet

I open very little of my server to the internet, and although I have a number of services running most of it is (where possible) proxied through Nginx.  I created a new server block for Atuin:

```nginx
server {
	listen 443 ssl;
	
	# Only allow access from VPN
	allow 192.168.2.0/24;
	deny all;

	ssl_certificate /usr/local/etc/ssl/atuin/fullchain.pem;
	ssl_certificate_key /usr/local/etc/ssl/atuin/privkey.pem;

	server_name atuin.manaha.co.uk;

	location / {
		proxy_pass http://172.16.0.10:8888/;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}


	access_log /usr/local/var/log/nginx/atuin.manaha.co.uk.log;
	error_log /usr/local/var/log/nginx/atuin.manaha.co.uk.error.log;
}
```

You'll see from above that I only allow access to the service from my VPN, further restricting access to the service.

# Atuin Client

Now to actually start using Atuin, I installed the client on my laptop using (Homebrew](https://brew.sh/) and added the following to my `~/.zshrc` file:

```shell
#####################
#      Atuin        #
#####################
if command -v atuin > /dev/null; then
	eval "$(atuin init zsh)"
fi
```

It's right at the end of my RC file at the moment, so it overrides anything else that might be set - I have a bunch of stuff in my `zshrc` (which can be found [here](https://github.com/forquare/dotfiles/blob/main/zshrc)), and I'm inclined to not tidy stuff up yet because I still have some machines where Atuin isn't installed.

I also needed to configure the client, here's what I changed from the default:

```toml
dialect = "uk"
sync_address = "https://atuin.manaha.co.uk"

## which filter mode to use
## possible values: global, host, session, directory
filter_mode = "host"

## which search mode to use when atuin is invoked from a shell up-key binding
## the accepted values are identical to those of "search_mode"
## leave unspecified to use same mode set in "search_mode"
search_mode_shell_up_key_binding = "prefix"

## the maximum number of lines the interface should take up
## set it to 0 to always go full screen
inline_height = 20

## use ctrl instead of alt as the shortcut modifier key for numerical UI shortcuts
## alt-0 .. alt-9
ctrl_n_shortcuts = true

## Defaults to true. If enabled, upon hitting enter Atuin will immediately execute the command. Press tab to return to the shell and edit.
# This applies for new installs. Old installs will keep the old behaviour unless configured otherwise.
enter_accept = true
```

I've included some of the comments for the less obvious settings.

# Thoughts

Before I say much else, I should probably point out that I've a very "vanilla" CLI user.  I use stock ZSH with some of my own customisations.  I don't use oh-my-zsh or anything fancy like that.  I generally use more traditional tooling and haven't yet found time/headspace to investigate some of the newer fancy tooling (e.g. I may use a mix of `find(1)` and `grep(1)` rather than something wizzy like ripgrep).  Much of this is because I've become used to working with a standard toolkit across multiple operating systems, and historically working on machines where installed software is tightly controlled.

I've been using Atuin for a few months now, and I'm certainly not making full advantage of it.  The most useful feature for me are the ability to sync my history to save it (I've found that ZSH occasionally overwrites the history file and thus truncates is).

There are a few things that niggle at me.  I've not checked for or written issues yet, partly because of lack of time (this blog post has taken four months), and partly because I'm wondering if they are just niggles that are systematic to the way I've used the command line for wo decades.  I'm also aware that the team behind Atuin is working hard at bringing in new features and processing a large influx of new users.  I'll try and summarise here some of the niggles I've felt, along with _my_ ideal behavour where appropriate:

* Complex search: I'm used to searching for things using regular expressions, however out of the [search modes](https://docs.atuin.sh/configuration/config/#search_mode) that Atuin provides the closest in "fuzzy" search, which isn't regex and is another set of syntax to learn.
  * Ideally I would love to have my searches use the "prefix" mode (which has been how I've searched though history for a few years now), but either be able to switch to a regex based search _or_ what would be cooler is if Atuin could figure out I've started writing regex.
* Searching in different contexts: Atuin supports searching within different [filter modes](https://docs.atuin.sh/configuration/config/#filter_mode) for instance across all history, only history recorded on the current host or specific directory, or in the current session.  This is very powerful, and it's obvious that `global` or `host` are going to be the obvious default choice for most instances, however if (like me) you work in multiple terminals at a time then you can find the history of one terminal session pollutes the history from another .  
  For example, if I type `ls` in one terminal, then switch to another terminal and type `cd /tmp`, then switch to the first terminal and press the up arrow, I'll see `cd /tmp` in the history rather than `ls`.  
  This is a pain because all of a sudden the set of commands I was closely working with in one session are burried further back in the history.
  * The obvious solution is to get used to switching filter modes more often, but _my_ ideal solution would be to have a combination of the `host`/`global` and `session` filter modes.  When searching `session` history would come first, and then a frequently updated `host` or `global` history would be appended after.  This would make my sesison history immediatly available, but also allow me to search a winder history if I needed to without switching modes.
* Sync issues: Recently Atuin v18 was released and changes the sync mechanism.  The FreeBSD binary package still hasn't been built and so my Atuin server is still at v17, but homebrew updated my laptop to v18.  I did know this would cause an issue, but I didn't realise it would be so prominant - an error printed every time I interacted with the shell (pressed "up", typed a command, etc) which was a pain to use.  
  It was simple enough to downgrade Atuin, but it would have been nice if I had a single error (maybe at the start of each session) and then be able to just get on with work.
* "Up issues": the [docs](https://docs.atuin.sh/configuration/key-binding/#disable-up-arrow) starte "_Our default up-arrow binding can be a bit contentious. Some people love it, some people hate it. Many people who found it a bit jarring at first have since come to love it, so give it a try!_" so I'm giving it a try and generally find it pretty pleasant.  My only complaint is the speed of it.  
  It is probably bad practice, but I often find myself mashing "up" three times to get my `git add .` command, then mashing another three times for `git commit -m "WIP"`, and then another three times for `git push` when fixing CI checks - or similarly for other combinations of commands.  Atuin doesn't stop this, but it does make the process slower, and after ~four months I haven't yet got out of the habit of typing commands like this, and not putting them on a single line

Are these niggles going to stop me using Atuin?  Absolutely not!  Maybe I'll change my ways, maybe Atuin will change to accomodate my existing muscle memory.

All in all, I would recommend you go and try it out!

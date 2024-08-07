---
date: 2019-01-06T14:33:43
title: "acme.sh revisited: ECC & Wildcards"
tags: ["Freebsd","acme.sh","let's encrypt"]
categories:  ["computing"]
---

A while ago I [wrote about using acme.sh](/2017/11/05/acme.sh-plus-linode-plus-dns-plus-freebsd/) to automate my HTTPS certificates.  
In the post I used a domain (bnix.club) along with a number of specific subdomains (“logs.bnix.club”, “f.bnix.club”, “www.bnix.club”).
<!--more-->
Today I wanted to add a subdomain to an existing domain: manaha.co.uk.  
This has a number of subdomains, so rather than adding a new one I decided to create a [wildcard certificate](https://github.com/Neilpang/acme.sh#11-issue-wildcard-certificates).  
While browsing the documentation for `acme.sh`, I came across [ECC certificates](https://github.com/Neilpang/acme.sh#10-issue-ecc-certificates), and thought that if I was recreating a certificate that I could use this too.

The process is *very* similar to the previous post, I’m putting this information here since it is a little different (different enough that I’ll forget what I did in the future...)  
I will cut out the output from each command this time, since it will largely be the same.

**Note:** All steps below were taken as the `acme` user.

## 0. Clean environment

Before I started this process, I cleaned out the old certificates and settings

	$ acme.sh --remove -d manaha.co.uk
	$ rm -rf /usr/local/etc/ssl/manaha/*
	$ rm -rf ~/certs/manaha.co.uk/

## 1. Issuing an ECC Wildcard certificate

	$ acme.sh --issue --dns dns_linode_v4 -d 'manaha.co.uk' -d '*.manaha.co.uk' --keylength ec-256

This issues a new certificate to `manaha.co.uk`, and all subdomains (wildcard - see the `*` in the second domain declaration).  It uses Linode DNS to verify I have control of the domains.  The `--keylength ec-256` part tells `acme.sh` to create an ECDSA certificate (prime256v1, "ECDSA P-256”).

## 2. Installing the certificate 

This uses the same mechanisms as in the previous post, so make sure you read that if you’re following along:

	$ acme.sh --install-cert --ecc -d 'manaha.co.uk' -d '*.manaha.co.uk' --key-file /usr/local/etc/ssl/manaha/privkey.pem --fullchain-file /usr/local/etc/ssl/manaha/fullchain.pem --reloadcmd "sleep 65 && touch /var/db/acme/.restart_nginx"

The only real difference between this post and the last one is the `--ecc`, this tells `acme.sh` that the certificate being used is ECDSA.

## 3. Renewing certificates 

This was already done for me, and it’s documented in the original post.

## 2021 Update

I've started using the Linode V4 API, so the issuing of an ECC wildcard certificate has been amended to reflect that.

---
date: 2024-07-08T15:38:00
title: "Sorry, that branch name is invalid."
tags: ["git", "github"]
categories: ["computing"]
---

Have you ever tried to create a new branch it GitHub and been met with the error:

> Sorry, that branch name is invalid.

Perhaps on GitLab you've seen the error that looks like this:

> Failed to create branch 'foo': invalid reference name 'master'

These errors are unclear.
<!--more-->

Gitea gives us an error that is more clear:

> Branch name "foo" conflicts with the already existing branch "foo/bar".

And the Git CLI also gives us a clearer understanding about what is happening:

> fatal: cannot lock ref 'refs/heads/foo': 'refs/heads/foo/bar' exists; cannot create 'refs/heads/foo'

The problem in these cases is that we have tried to create a branch `foo` when another branch (`foo/bar`) already exists.

Internally, Git stores branches as files in the `.git/refs/heads` directory. When we try to create a branch, Git will try to create a file in this directory with the name of the branch we are trying to create. If the branch name we are trying to create matches a directory that already exists, Git will not be able to create the branch.


It's like trying to do the following:

```shell
# Make a new directory called foo
$ mkdir foo
# Create a file called foo/bar that contains a dummy git hash
$ echo '79f963f8601f5e78f786730933c2320fdcbda58f' > foo/bar

# Now try to create a file called foo
$ echo '74b1d6cbb944b4defb49b983e798796ed6a161ca' > foo
```

Your shell will return an error like:

```shell
zsh: is a directory: foo
```

Because you cannot create a file with the same name as a directory that already exists.

If you want to create a branch called `foo`, you will need to delete the branch `foo/bar` (and any other nested branches) first.

**It is important to realise that the reverse is also true** - if you have a branch called `foo` and you want to create a branch called `foo/bar`, you will need to delete the branch `foo` first.

This came up at work today when a colleague was trying to create a branch called `develop` but had multiple branches under `develop/` already. Searching for the GitHub error returned zero results on a number of search engines, so if you get this error hopefully this has helped you!

---
Title: Git - Install GIT via PPA on Ubuntu
Description: Git - Install GIT via PPA on Ubuntu
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### Adding the `git-core` PPA to the system.

First install `python-software-properties` like this:

    sudo aptitude install python-software-properties

Next add the WebUpd8 PPA repository to the system like this:

    sudo add-apt-repository ppa:git-core/ppa

With that done, run `aptitude update` like this:

    sudo aptitude update

### Installing Git.

If you already have `git` installed, running `aptitude upgrade` will upgrade your core install:

    sudo aptitude upgrade

Or if you have not instaleld it yet, just install it via a standard `aptitude install` command and you should be good to go:

    sudo aptitude install git

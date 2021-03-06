---
Title: Netatalk
Description: A cheat sheet for Netatalk related items.
Author: Jack Szwergold
Date: 2015-09-19
Robots: noindex,nofollow
Template: index
---

Netatalk is a very cool, open source implementation of Apple’s AppleTalk networking protocols. Very nice and useful for setting up Linux/Unix AFP file servers.

***

### Installing Netatalk.

Install `netatalk` via `aptitude` like this:

    sudo aptitude install netatalk

***

### Start, stop and control Netatalk.

    sudo service netatalk start
    sudo service netatalk stop
    sudo service netatalk restart
    sudo service netatalk force-reload

***

### Sundry Netatalk items.

Edit the `netatalk` config file:

    sudo nano /etc/netatalk/afpd.conf

Add this line to the end of the config file:

    - -tcp -noddp -uamlist uams_dhx.so,uams_dhx2_passwd.so -nosavepassword

Edit the `AppleVolumes` list:

    sudo nano /etc/netatalk/AppleVolumes.default

Add this line to the end of the `AppleVolumes.default` config file to define a Netatalk mount point:

    # /var/www  WebRoot allow:@www-readwrite options:usedots,upriv
    /var/www  "WebRoot (Netatalk)" allow:@www-readwrite options:usedots,upriv,noadouble

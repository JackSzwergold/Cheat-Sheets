---
Title: Sysstat
Description: A cheat sheet for Sysstat related items.
Author: Jack Szwergold
Date: 2015-09-16
Robots: noindex,nofollow
Template: index
---

### Install Sysstat.

Install the Sysstat utilities to monitor system statistics via `aptitude`:

    sudo aptitude install sysstat

***

### Basic `munin` usage items.

Control `sysstat`:

    sudo service sysstat reload
    sudo service sysstat restart
    sudo service sysstat start
    sudo service sysstat stop

***

### Clearing up “sadc not enabled…” errors.

If you get this message:

> sadc not enabled in `/etc/default/sysstat`, not starting.

Then edit this file:

    sudo nano /etc/default/sysstat

Then look for this and change the value of `ENABLED` from `false` to `true`:

    # Should sadc collect system activity informations? Valid values
    # are "true" and "false". Please do not put other values, they
    # will be overwritten by debconf!
    ENABLED="true"

***

### Clearing up “Invalid system activity file…” errors.

If you get a message like the following:

> Invalid system activity file:` /var/log/sysstat/sa14`

Then just purge and re-install `sysstat` to get it back up and running:

    sudo aptitude purge sysstat

---
Title: Ubuntu - Setting Up and Using Apple Filing Protocol Utility Items
Description: Ubuntu - Setting Up and Using Apple Filing Protocol Utility Items
Author: Jack Szwergold
Date: 2015-10-04
Robots: noindex,nofollow
Template: index
---

### Installation.

Install the Apple Filing Protocol (AFP) stuff:

    sudo aptitude install afpfs-ng-utils

### Mounting and unmounting an AFP volume.

Mounting an AFP volume:

    mount_afp afp://user:password@192.168.1.101/sharename /media/AFP

The overall structure of how to mount an AFP volume:

    afp_client mount -u [user_name] -p [password] [server]:[volume] [mountpoint]

Unmount an AFP volume:

    afp_client unmount [mountpoint]

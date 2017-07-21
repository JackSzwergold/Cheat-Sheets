## Ubuntu - Setting Up and Using Apple Filing Protocol Utility Items

By Jack Szwergold

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

***

*Ubuntu - Setting Up and Using Apple Filing Protocol Utility Items (c) by Jack Szwergold; written on October 4, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
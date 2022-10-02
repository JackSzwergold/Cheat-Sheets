---
Title: Linux-Unix - HFS Volume Related Items
Description: A cheat sheet for HFS volume related items.
Author: Jack Szwergold
Date: 2015-09-22
Robots: noindex,nofollow
Template: index
---

### Sundry stuff.

Shows a list of all connected block level devices using `lsblk`:

    lsblk

Shows all connected & mounted devices with `fdisk`:

    sudo fdisk -l

List all USB devices with the `-v` verbose flag:

    lsusb -v

### HFS specific stuff.

Make sure that you have hfsprogs installed. Example installation command:

    sudo aptitude install hfsprogs

Mount or remount the HFS+ drive; commands need to be something like the following:

    sudo mount -t hfsplus -o force,rw /dev/sdx# /media/mntpoint

    sudo mount -t hfsplus -o remount,force,rw /dev/sdx# /mount/point

Finally, if the drive was improperly unmounted or has otherwise become partially corrupted run fsck.hfsplus (provided here by Jayson) as such:

    sudo fsck.hfsplus /dev/sdx#

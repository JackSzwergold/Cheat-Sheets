---
Title: Different Ways of Wiping Disk Space
Description: A cheat sheet for different ways of wiping disk space related items.
Author: Jack Szwergold
Date: 2015-10-08
Robots: noindex,nofollow
Template: index
---

## Different Ways of Wiping Disk Space

By Jack Szwergold

Zero out free space on a volume like this:

    cat /dev/zero >> /Volumes/Untitled/junk &

Write out random junk to the free space on a volume like this:

    cat /dev/urandom >> /Volumes/Untitled/junk_random &

***

*Different Ways of Wiping Disk Space (c) by Jack Szwergold; written on October 8, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
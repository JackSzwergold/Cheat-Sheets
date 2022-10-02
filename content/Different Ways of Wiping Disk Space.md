---
Title: Different Ways of Wiping Disk Space
Description: A cheat sheet for different ways of wiping disk space related items.
Author: Jack Szwergold
Date: 2015-10-08
Robots: noindex,nofollow
Template: index
---

Zero out free space on a volume like this:

    cat /dev/zero >> /Volumes/Untitled/junk &

Write out random junk to the free space on a volume like this:

    cat /dev/urandom >> /Volumes/Untitled/junk_random &

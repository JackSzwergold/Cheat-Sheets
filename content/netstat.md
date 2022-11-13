---
Title: Netstat
Description: A cheat sheet for Netstat related items.
Author: Jack Szwergold
Date: 2017-11-09
Robots: noindex,nofollow
Template: index
---

#### Some basic usage examples.

Use `watch` to check the number of established connections at an interval of 1 second:

    watch -n 1 "netstat -an | grep ESTABLISHED | wc -l"

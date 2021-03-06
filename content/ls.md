---
Title: LS
Description: A cheat sheet for LS related items.
Author: Jack Szwergold
Date: 2015-09-17
Robots: noindex,nofollow
Template: index
---

List one item per line:

    ls -1

List items in a one line, comma separated list format:

    ls -m

List all of the items in the directory, order it by time and reverse the list:

    ls -latr

List all of the items in the directory in natural order and in reverse:

    ls -lavr

Do a directory search in reverse and find the last file with the `.sh` in the filname; can be changed to match anything else in the filename:

    ls -lrt | awk '/.sh/ { filename=$NF }; END { print filename }'

List only directories:

    ls -l | egrep '^d'

List only files:

    ls -l | egrep -v '^d'

Get a count of files (not hidden) in a folder:

    ls -1 | wc -l

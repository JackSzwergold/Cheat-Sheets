---
Title: Find - User and Permissions Related Items
Description: A cheat sheet for Find related user and permissions items.
Author: Jack Szwergold
Date: 2015-09-28
Robots: noindex,nofollow
Template: index
---

## Find - User and Permissions Related Items

By Jack Szwergold

### Change user ownership on a directory.

Dry run of the command using `echo`:

	find '/var/www' -user some_user |\
	  while read FILENAME
	  do
	    echo "${FILENAME}"
	  done

Actual, functional script:

	sudo find '/var/www' -user some_user |\
	  while read FILENAME
	  do
	    sudo chown another_user "${FILENAME}"
	  done

### Find items without a user and assign a new user.

Dry run of the command using `echo`:

	find '/var/www' -nouser |\
	  while read FILENAME
	  do
	    echo "${FILENAME}"
	  done

Actual, functional script:

	sudo find '/var/www' -nouser |\
	  while read FILENAME
	  do
	    sudo chown some_user "${FILENAME}"
	  done

***

*Find - User and Permissions Related Items (c) by Jack Szwergold; written on September 28, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*

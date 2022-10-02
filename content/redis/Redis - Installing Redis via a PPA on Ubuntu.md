---
Title: Redis - Installing Redis via a PPA on Ubuntu
Description: Redis - Installing Redis via a PPA on Ubuntu
Author: Jack Szwergold
Date: 2015-09-22
Robots: noindex,nofollow
Template: index
---

### Get rid of the default Ubuntu repository installed version of Redis.

Just run this `sudo aptitude purge` command to get rid of any Redis stuff installed via the default Ubuntu repository:

    sudo aptitude purge redis-server

### Adding the Chris Lea PPA to the system.

First install `python-software-properties` like this:

    sudo aptitude install python-software-properties

Next add the Chris Lea PPA repository to the system like this:

	sudo add-apt-repository ppa:chris-lea/redis-server
	
With that done, run `aptitude update` to get the new repository picked up like this:

	sudo aptitude update
	
Then install Redis via `aptitude` like this:

	sudo aptitude install redis-server

Check the version number like this:

	redis-server --version

And the output should be something like this:

	Redis server v=3.0.4 sha=00000000:0 malloc=jemalloc-3.6.0 bits=64 build=e10d4bb04434c274

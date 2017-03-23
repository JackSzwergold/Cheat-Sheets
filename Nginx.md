## Nginx

By Jack Szwergold

Get the version of Nginx from the command line:

    nginx -v

If installed in a non-standard location—like `/opt`—run it like this:

    /opt/nginx/sbin/nginx -v

The output should be something like this:

    nginx version: nginx/1.4.1

Install `ngxtop` to monitor realtime stats on the server:

	sudo aptitude install ngxtop

***

*Nginx (c) by Jack Szwergold; written on September 27, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*

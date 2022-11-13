---
Title: Screen
Description: A cheat sheet for Screen related items.
Author: Jack Szwergold
Date: 2015-09-20
Robots: noindex,nofollow
Template: index
---

Install `screen`:

    sudo aptitude install screen

Create a screen named `test_screen`.

    screen -S test_screen

List all open screens.

    screen -ls

The output would be.

    There is a screen on:
            2839.test_screen        (09/27/2015 02:48:00 AM)        (Attached)
    1 Socket in /var/run/screen/S-sysop.

So if you wanted to reattach to that session, just enter this line.

    screen -D -r '2839.test_screen'

The `-D` detaches the screen session and then the `-r` reattaches the session.

---
Title: Amazon - Amazon EC2 SSH Adjustments to Allow Password Authentication
Description: A cheat sheet for Amazon EC2 SSH adjustments to allow password authentication.
Author: Jack Szwergold
Date: 2016-01-17
Robots: noindex,nofollow
Template: index
---

Open up the SSH daemon config like this:

    sudo nano /etc/ssh/sshd_config

Find the line for `PasswordAuthentication` which will look like this:

    # Change to no to disable tunnelled clear text passwords
    PasswordAuthentication no

Change that `PasswordAuthentication` value to `yes` like this:

    # Change to no to disable tunnelled clear text passwords
    PasswordAuthentication yes

    sudo service ssh restart

And once you have another user setup with `sudo` rights, it’s safe to get rid of that default `ubuntu` user:

    sudo deluser --remove-home ubuntu

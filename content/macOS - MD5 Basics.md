---
Title: macOS - MD5 Basics
Description: A cheat sheet for MD5 basics.
Author: Jack Szwergold
Date: 2015-09-14
Robots: noindex,nofollow
Template: index
---

Get the MD5 of a file via this command; change `.bash_profile` to match the actual path and file name of the file you want to check:

    md5 .bash_profile

The returned output `md5` command should look something like this:

    MD5 (.bash_profile) = da4b8a88ad68b6c11a05e4956677c73e

If you need a simpler form of output just run `md5` with the `-r` command like this:

    md5 -r .bash_profile

The output would look something like this which is cleaner and nicer to use for some programming purposes:

    da4b8a88ad68b6c11a05e4956677c73e .bash_profile

You can also use the `openssl` command to get an MD5 like this:

    openssl md5 .bash_profile

The returned output of that `openssl` command should look something like this:

    MD5(.bash_profile)= da4b8a88ad68b6c11a05e4956677c73e

---
Title: macOS - ID3Lib
Description: A cheat sheet for ID3Lib related items.
Author: Jack Szwergold
Date: 2016-08-22
Robots: noindex,nofollow
Template: index
---

This tutorial explains how you can install ID3Lib directly from source code on macOS. Tested on macOS 10.11.5.

### Installing ID3Lib via official source code archive.

First clone the source code from attilagyorffy’s `id3lib-osx` GitHub repository:

    git clone git@github.com:attilagyorffy/id3lib-osx.git
    
Now go into the repository directory:

    cd id3lib-osx
    
Run this `configure` command:

    ./configure
    
Once the `configure` process completes, run `make`:

    make
    
Finally install it by running `sudo make install`:

    sudo make install

### Binaires installed.

* `id3info`
* `id3tag`
* `id3convert`
* `id3cp`

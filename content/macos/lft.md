---
Title: macOS - LFT
Description: A cheat sheet for LFT related items.
Author: Jack Szwergold
Date: 2015-09-14
Robots: noindex,nofollow
Template: index
---

This tutorial explains how you can install `lft` directly from source code on macOS. Tested on macOS 10.9.5.

First grab a compressed archive from an official `lft` source site:

    curl -o ./lft-3.73.tar.gz -O http://pwhois.org/dl/index.who?file=lft-3.73.tar.gz

Next, decompress the archive like this:

    tar -xf lft-3.*.tar.gz

Now go into the decompressed directory:

    cd lft-3.*
    
Run this `configure` command:

    ./configure

Once the `configure` process completes, run `make`:

    make

Finally install it by running `sudo make install`:

    sudo make install

And once it’s installed, run the command with `-v` to do a simple check to see it’s working:

    lft -v

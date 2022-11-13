---
Title: macOS - Recode
Description: A cheat sheet for Recode related items.
Author: Jack Szwergold
Date: 2015-11-24
Robots: noindex,nofollow
Template: index
---

Recode is a character set conversion tool.

First clone the source code from the official Recode GitHub repository:

    git clone https://github.com/pinard/Recode.git
    
Now go into the repository directory:

    cd Recode

Then run this `configure` command:

    ./configure
    
Once the `configure` process completes, run `make`:

    make
    
Finally install it by running `sudo make install`:

    sudo make install

To check if Recode was properly installed, just check the version like this:

    recode --version

The returned output should be:

    recode 3.7-beta2
    Written by Franc,ois Pinard <pinard@iro.umontreal.ca>.
    
    Copyright (C) 1990, 92-94, 96, 97, 99, 08 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

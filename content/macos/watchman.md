---
Title: macOS - Watchman
Description: A cheat sheet for Watchman related items.
Author: Jack Szwergold
Date: 2015-11-18
Robots: noindex,nofollow
Template: index
---

Watchman is a tool created by Facebook that is described as follows, “Watchman exists to watch files and record when they change. It can also trigger actions (such as rebuilding assets) when matching files change.” The main reason I use it is for improved development with EmberJS which recommends using Watchman over built-in file watching services on macOS. So hey, here is how I install it so I can do that.

First clone the source code from the official Watchman GitHub repository:

    git clone https://github.com/facebook/watchman.git
    
Now go into the repository directory:

    cd watchman

Checkout the latest tag of the source code like this:

    git checkout v4.7.0

Run `autogen.sh`:

    ./autogen.sh
    
Then run this `configure` command:

    ./configure
    
Once the `configure` process completes, run `make`:

    make
    
Finally install it by running `sudo make install`:

    sudo make install

To check if Watchman was properly installed, just check the version like this:

    watchman --version

The returned output should be:

    4.1.0

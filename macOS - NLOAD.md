## macOS - NLOAD

By Jack Szwergold

This tutorial explains how you can install `nload` directly from source code on macOS. Tested on macOS 10.9.5.

#### Installing `nload` via official source code archive.

First grab a compressed archive from an official `nload` source site:

    curl -O -L http://www.roland-riegel.de/nload/nload-0.7.4.tar.gz

Next, decompress the archive like this:

    tar -xf nload-0.*.tar.gz

Now go into the decompressed directory:

    cd nload-0.*

Once in that directory, open `configure.in` to modify it for macOS (Darwin) use:

    nano configure.in

Look for the line that has `*bsd*` in it like this:

    *bsd*)

And change that `*bsd*` to be `*darwin*` (aka: macOS) like this:

    *darwin*)

After that’s done, find this line:

    test "$enableval" = "no" && LDFLAGS="$LDFLAGS -s",

And change it to remove the `-s` like this:

    test "$enableval" = "no" && LDFLAGS="$LDFLAGS",

Close and save that file and now load this other file:

    nano src/devreader-bsd.cpp

Look for this line:

	interfaceNames.push_back(string(sdl->sdl_data));

And change it to:

	interfaceNames.push_back(string(sdl->sdl_data).substr(0, sdl->sdl_nlen));

Save that file and now run `run_autotools`:

    ./run_autotools

Run this `configure` command:

	./configure --disable-debug --disable-dependency-tracking
	
Once the `configure` process completes, run `make`:

	make
	
Finally install it by running `sudo make install`:

	sudo make install

And once it’s installed, run the command with `--version` to do a simple check to see it’s working:

	nload --help

Response should be something like this:

    nload version 0.7.4

Uninstall if you need to:

	sudo make uninstall

***

*macOS - NLOAD (c) by Jack Szwergold; written on September 15, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
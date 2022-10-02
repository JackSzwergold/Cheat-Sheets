---
Title: macOS - SSHPASS
Description: A cheat sheet for SSHPASS related items.
Author: Jack Szwergold
Date: 2016-09-15
Robots: noindex,nofollow
Template: index
---

## macOS - SSHPASS

By Jack Szwergold

    curl -O http://heanet.dl.sourceforge.net/project/sshpass/sshpass/1.06/sshpass-1.06.tar.gz
    tar -xf sshpass-1.06.tar.gz
    cd sshpass-1.*
    ./configure
    make
    sudo make install
    sudo make uninstall

	rm -f /usr/local/bin/sshpass
	rm -f /usr/local/share/man/man1/sshpass.1

***

*macOS - SSHPASS (c) by Jack Szwergold; written on September 15, 2016. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
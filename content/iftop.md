---
Title: IFTop
Description: A cheat sheet for IFTop related items.
Author: Jack Szwergold
Date: 2015-09-14
Robots: noindex,nofollow
Template: index
---

### Basic IFTop usage.

You can just launch IFTop with no options and it will show network traffic on the main network interface such as `en0` (macOS) or `eth0` (Linux):

    sudo iftop

Show the network traffic on a specific network interface with the `-i` flag:

    iftop -i en0

Show the network traffic on a specific network interface with the `-i` flag and filter out sundry DNS request traffic:

    sudo -i en0 iftop -f 'not port domain'

Check for port 80 (HTTP) traffic on `en0`:

    sudo iftop  -i en0 -nBP -f 'port 80'

Check for port 548 (HTTP) traffic on `en0`:

    sudo iftop  -i en0 -nBP -f 'port 548'

Check for port 445 (HTTP) traffic on `en0`:

    sudo iftop  -i en0 -nBP -f 'port 445'

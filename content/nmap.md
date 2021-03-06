---
Title: Nmap
Description: A cheat sheet for Nmap related items.
Author: Jack Szwergold
Date: 2015-10-03
Robots: noindex,nofollow
Template: index
---

Scan a range via a wildcard and returns hostname as well as IP:

    nmap -sP 192.168.1.*

Scan a range via a slash notation and returns hostname as well as IP:

    nmap -sP 192.168.1.1/24

Scan a range via a slash notation and checks if ports `80` and `8080` are open and returns hostname as well as IP:

    sudo nmap -v -O 192.168.1.1/24 -p80,8080

Scans all ports on `192.168.1.1` in verbose mode using the `-v` flag:

    nmap -v 192.168.1.1

Scans all ports on `192.168.1.1` in verbose mode using the `-v` flag and attempts to detect OS via the `-O` flag:

   sudo nmap -v -O 192.168.1.1

Scanning multiple hosts with grepable output to simply show port status:

    nmap -p 27017 -oG - example1.com example2.com example3.com example4.com | grep Ports

***

### Port scanning a specific host.

A standard TCP port scan:

    sudo nmap -sS -Pn -p- -T4 -vv --reason -oN ~/nmap.TCP.results example.com

A standard UDP port scan:

    sudo nmap -sU -Pn -p- -T4 -vv --reason -oN ~/nmap.UDP.results example.com

A scan designed to attempt to discover versions of services on the server:

    sudo nmap -sV -Pn -p 22,80 -vv --reason -oN ~/nmap.SERVICE.results example.com

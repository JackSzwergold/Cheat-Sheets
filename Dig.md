## Dig

By Jack Szwergold

### Basic Dig commands.

Detailed DNS info for a hostname:

    dig +nocmd [hostname] any +multiline +noall +answer

Look up the TTL for a hostname:

    dig +nocmd [hostname] +noall +answer

Look up the authoritative `NS` (namservers) for a hostname:

    dig NS [hostname]

DNS lookup of a hostname on a specific nameserver:

    dig @[dns server address] NS [hostname]

DNS lookup of a hostname on a specific nameserver using OpenDNS’s DNS servers:

	dig @208.67.222.222 NS [hostname]
	dig @208.67.222.220 NS [hostname]

DNS lookup of a hostname on a specific nameserver using Google’s DNS servers:

	dig @8.8.8.8 NS [hostname]
	dig @8.8.4.4 NS [hostname]

Get a list of all DNS data connected to a domain. But this doesn’t work nowadays since most all DNS servers out there deny zone transfers:

    dig [hostname] -t AXFR

Get all records connected to a domain:

    dig [hostname] ANY

Get pretty much all available info on a domain:

    dig +nocmd [hostname] any +multiline +noall +answer

***

*Dig (c) by Jack Szwergold; written on September 15, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
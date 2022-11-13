---
Title: Ubuntu - LDAP Related Items
Description: Ubuntu - LDAP Related Items
Author: Jack Szwergold
Date: 2015-10-05
Robots: noindex,nofollow
Template: index
---

Install `ldap-utils` via `aptitude` like this:

    sudo aptitude install ldap-utils

Run an LDAP search like this through the command line:

    ldapsearch -x -h ad.example.com -D "someuser@EXAMPLE.COM" -b "DC=someDC,DC=EXAMPLE,DC=COM" -W -L "sAMAccountName=some_user"

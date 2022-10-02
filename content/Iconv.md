---
Title: Iconv
Description: A cheat sheet for Iconv related items.
Author: Jack Szwergold
Date: 2015-10-07
Robots: noindex,nofollow
Template: index
---

The overall format of using Incov from the command line:

	iconv -f old-encoding -t new-encoding file.txt > file_converted.txt

An exmaple of converting a file from `UTF-8` to `iso-8859-1`:

	iconv -f UTF-8 -t iso-8859-1 file.txt > file_converted.txt

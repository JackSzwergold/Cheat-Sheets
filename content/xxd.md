---
Title: XXD
Description: A cheat sheet for XXD related items.
Author: Jack Szwergold
Date: 2015-09-11
Robots: noindex,nofollow
Template: index
---

`xxd` is a nice little hexdump (hexadecimal dump) utility available on most Linux/Unix systems. It allows you to get a hexdump of a file or even standard input.

### Hexdump the contents of the file.

Here is an example of a simply hexadecimal dump of a file:

    xxd ~/.bash_profile

Since `xxd` basically streams a file input into a hexdump output, a huge file can result in an endless hexdump. To avoid that just pipe the output to `more` to add a pause to the output stream like this:

    xxd ~/.bash_profile | more

***

### Bitdump the contents of the file.

In addition to a hexdump (hexadecimal dump), `xxd` can also do a bitdump (binary digits) by using the `-b` option like this:

    xxd -b ~/.bash_profile

And you can also pipe the output to `more` to add a pause to the output stream like this:

    xxd -b ~/.bash_profile | more

***

### Get cleaner a cleaner bitdump the contents of the file.

Let’s say you just want the core bitdump of a file. You can combine a bitdump of a file with `head` and `cut` to get cleaner output:

    xxd -b ~/.bash_profile | head | cut -b10-62

---
Title: Awk
Description: A cheat sheet for Awk related items.
Author: Jack Szwergold
Date: 2015-10-08
Robots: noindex,nofollow
Template: index
---

If you need to concatenate a whole bunch of files and a newline between each file’s content—so the concatenation is clean—run an Awk command like this:

    awk 'FNR==1{print ""}1' *.txt > output_files.txt

Just change `*.txt` to match the text file pattern you are attempting to match and change the `output_files.txt` filename as well.

This command will take a text file and will convert all uppercase to capital case:

    cat foo.txt | tr "[A-Z]" "[a-z]"  | awk '{for (i=1;i<=NF;i++) $i=toupper(substr($i,1,1)) substr($i,2)} 1' > bar.txt

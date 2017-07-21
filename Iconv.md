## Iconv

By Jack Szwergold

***

The overall format of using Incov from the command line:

	iconv -f old-encoding -t new-encoding file.txt > file_converted.txt

An exmaple of converting a file from `UTF-8` to `iso-8859-1`:

	iconv -f UTF-8 -t iso-8859-1 file.txt > file_converted.txt

***

*Iconv (c) by Jack Szwergold; written on October 7, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
---
Title: Find - Text Processing Items
Description: A cheat sheet for Find related text processing items.
Author: Jack Szwergold
Date: 2021-05-19
Robots: noindex,nofollow
Template: index
---

This simple script uses `tr`:

      tr -d '\r' < "${source_filepath}" > "${source_filepath}".txt;

### Convert old Mac OS line endings to modern text files using `dos2unix`.

And here you go:

    find -E . -type f |\
      while read source_filepath
      do
        dos2unix -k -n -c mac "${source_filepath}" "${source_filepath}".txt;
      done

### Lowercase file names and change spaces to underscores.

    find . -depth -name "* *" -type f |\
      while read source_filepath
      do
        destination_filepath=$(echo "${source_filepath}" | tr A-Z a-z | tr -s ' ' | tr ' ' '_'| sed 's/_\./\./');
        mv -f "${source_filepath}" "${destination_filepath}";
      done


---
Title: Find - Text Processing Items
Description: A cheat sheet for Find related text processing items.
Author: Jack Szwergold
Date: 2021-05-19
Robots: noindex,nofollow
Template: index
---

This simple script uses `tr`:

      tr -d '\r' < "${full_text_filepath}" > "${full_text_filepath}".txt;

### Convert old Mac OS line endings to modern text files using `dos2unix`.

And here you go:

    find -E "Desktop/Text" -type f |\
      while read full_text_filepath
      do
        dos2unix -k -n -c mac "${full_text_filepath}" "${full_text_filepath}".txt;
      done

### Lowercase file names and change spaces to underscores.

    find . -depth -name "* *" -type f |\
      while read full_text_filepath
      do
        new_filename=$(echo "${full_text_filepath}" | tr A-Z a-z | tr -s ' ' | tr ' ' '_'| sed 's/_\./\./');
        mv -f "${full_text_filepath}" "${new_filename}";
      done


## Git - Changing Text Across Multiple Commit Messages

By Jack Szwergold

### Changing text across multiple commit messages in a branch.

Do a search of commit messages to find the pattern you want to change.

    git log --oneline --grep="things"

Filter and run `sed` to change all occurrences of “things” to “stuff.”

    git filter-branch -f --msg-filter "sed 's/things/stuff/'" HEAD

Now push that commit back to GitHub.

    git push -f

***

*Git - Changing Text Across Multiple Commit Messages (c) by Jack Szwergold; written on September 15, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
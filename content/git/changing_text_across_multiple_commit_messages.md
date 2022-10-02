---
Title: Git - Changing Text Across Multiple Commit Messages
Description: Git - Changing Text Across Multiple Commit Messages
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### Changing text across multiple commit messages in a branch.

Do a search of commit messages to find the pattern you want to change.

    git log --oneline --grep="things"

Filter and run `sed` to change all occurrences of “things” to “stuff.”

    git filter-branch -f --msg-filter "sed 's/things/stuff/'" HEAD

Now push that commit back to GitHub.

    git push -f

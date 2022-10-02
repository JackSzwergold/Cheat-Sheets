---
Title: Git - Removing a File from all Commits in a Branch
Description: Git - Removing a File from all Commits in a Branch
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### How to remove a file from every commit in a branch.

This will effectively rewrite every commit to remove the `markdown/things/index.md`.

    git filter-branch --tree-filter 'rm -f markdown/things/index.md' HEAD

But that might leave a final commit that would still show the file’s existence and contents. So this command might be more effective if you just want to rewrite the Git history completely:

	git filter-branch --force --index-filter \
	'git rm --cached --ignore-unmatch markdown/things/index.md' \
	--prune-empty --tag-name-filter cat -- --all

Now push that commit back to GitHub.

    git push -f

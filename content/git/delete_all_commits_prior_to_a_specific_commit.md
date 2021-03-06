---
Title: Git - Delete All Commits Prior to a Specific Commit
Description: Git - Delete All Commits Prior to a Specific Commit
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### How to delete all commits prior to a specific commit hash.

First, figure out what the commit hash is. In this case, let’s say it’s `11dae100c026c3ca43db973f752f25390e6aa8f6`. Now, remove the origin directory to make sure we don’t potentially re-add removed items:

    git remote rm origin

Add that `11dae100c026c3ca43db973f752f25390e6aa8f6` has to the `.git/info/grafts` like this:

    nano .git/info/grafts

And add the hash into that file and save it:

    11dae100c026c3ca43db973f752f25390e6aa8f6

Now run `filter-branch` to get the everything sorted out:

    git filter-branch -f -- --all

After that is done, remove the `.git/info/grafts` file like this:

    rm .git/info/grafts

Readd the `origin` like this:

    git remote add origin git@github.com:JackSzwergold/Colorspace-Conversions.git

Finally, push the newly rebased repo back to `origin` like this:

    git push -f --set-upstream origin master

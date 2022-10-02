---
Title: Git - Restoring a Deleted File in a Git Repository
Description: Git - Restoring a Deleted File in a Git Repository
Author: Jack Szwergold
Date: 2015-10-02
Robots: noindex,nofollow
Template: index
---

So let’s say you accidentally deleted a file in a Git repository and somehow you never noticed until a pile of commits in. Don’t panic! As long as you know the exact path of the file removed, you can recover it. Here is how.

First find the commit hash for the commit where the file was removed with the following command; be sure to change `[path of deleted file]` to the actual path of the deleted file:

	git rev-list -n 1 HEAD -- [path of deleted file]

Once you get that commit hash, run this command to restore the file; be sure to change `[commit hash]` to the actual commit hash and change `[path of deleted file]` to the actual path of the deleted file:

    git checkout [commit hash]^ -- [path of deleted file]

After that command is run, the file will be restored to the exact path where it was removed initially.
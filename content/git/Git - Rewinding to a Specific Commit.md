---
Title: Git - Rewinding to a Specific Commit
Description: Git - Rewinding to a Specific Commit
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### Rewinding to a specific commit hash in the repository.

Check the log for the commit hash.

    git log

Find a commit to roll back.

	commit b331419ad4cc4397a3138d3fc1166be014debc4f
	Author: Firstname Lastname <email_address@example.com>
	Date:   Sat May 16 21:31:20 2015 -0400
	
	    Setting a short name for the application; useful for deployments.

Reset the repo to that commit.

    git reset --hard b331419ad4cc4397a3138d3fc1166be014debc4f

Now push that commit back to GitHub.

    git push -f

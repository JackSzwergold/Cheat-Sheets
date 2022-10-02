---
Title: Git - Duplicating or Renaming a Repository
Description: Git - Duplicating or Renaming a Repository
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### How to duplicate and rename a repository.

First step is to create a new repo on GitHub.

Make a bare clone of the source repository.

    git clone --bare --recursive git@github.com:JackSzwergold/Memories.git

Go into that directory.

    cd Memories.git

Now push (via mirror) into the new repo.

    git push --mirror git@github.com:JackSzwergold/Writing.git

If any `git clone` copies s of the repo exist somewhere, change the origin URL this way.

    git remote set-url origin git@github.com:JackSzwergold/Writing.git
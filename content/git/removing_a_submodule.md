---
Title: Git - Removing a Submodule
Description: Git - Removing a Submodule
Author: Jack Szwergold
Date: 2015-09-15
Robots: noindex,nofollow
Template: index
---

### Removing a submodule from a git repository.

In this example, let’s remove the submodule `wordpress`. First, check the submodule status from the repository:

    git submodule status

Now, `deinit` the submodule from GIT:

    git submodule deinit wordpress

Now remove the actual `wordpress` directory:

    git rm -rf wordpress

And finally—if you have no need for any submodules in your repository—remove the `.gitmodules` file from the root of the repository:

    git rm .gitmodules

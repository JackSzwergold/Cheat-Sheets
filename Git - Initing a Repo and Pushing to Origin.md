## Git - Initing a Repo and Pushing to Origin

By Jack Szwergold

### Install GIT and get your local config setup.

For GitHub to work correctly with your email address and username, you should make sure to set your local email address and name on your system after your initial install.

This can be done by just running these `git config --global` commands:

	git config --global user.email "email_address@example.com"
	git config --global user.name "Firstname Lastname"

To confirm those values are set, just run those commands again without any set values like this:

    git config --global user.email
    git config --global user.name

And you can also set your system’s default git editor like this; I prefer to use `nano` for example:

	git config --global core.editor "nano"

Similarly, you can check if that value was set by running that command without an set value as so:

	git config --global core.editor

Or if you wish you can just directly create/edit the `.gitconfig` file like this:

	sudo nano ~/.gitconfig
	
	[user]
	        name = Firstname Lastname
	        email = email_address@example.com
	[core]
	        editor = nano

### Check your connection to GitHub.

Now, you need to have GIT installed in your machine, a GitHub account setup as well as have SSH kets setup between your working machine and GitHub.

The first thing you should do is check your connection to GitHub so you can get the hostkey setup properly by using this command:

    ssh -T git@github.com

If this is the first time you are running that command on your system the response will be something like:

	The authenticity of host 'github.com (192.30.252.131)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)?

Your response should be “yes” so that host authenticity for `github.com` can be added to your `~/.ssh/known_hosts`.

Then after you have done that, check your connection to GitHub again using the same `ssh -T` command as before:

    ssh -T git@github.com

And the response should be something like:

	Hi JackSzwergold! You've successfully authenticated, but GitHub does not provide shell access.

### Init the repository.

First go into your project directory like this:

	cd my_project

And init it as a repo like this:

    git init

### Create and add a `.gitignore` file to the repository.

Create a `.gitignore` file to set what files to ignore:

    nano .gitignore

For example, on Mac OS X we have no need to commit `.DS_Store` files so add `.DS_Store` to that file like this:

	.DS_Store

Next, add a `.gitignore` file to the repo with `git add` like this:

	git add .gitignore

Now, commit it with a message like this:

	git commit -a -m "Adding '.gitignore' to the repo."

### Amending a commit message.

Let’s say you made a mistake in the commit message you just made. You can amend it—basically redo it—like this:

	git commit --amend -m "Adding '.gitignore' to the repo."

Or perhaps the author name on the commit is incorrect. Just fix that by running a command like:

    git commit --amend --author="Jack Szwergold <JackSzwergold@gmail.com>"

And if you have already pushed the previous commit to a repository, you need to force push it like this:

    git push -f

### Add all files to the repository.

Now add all files—new and modified—by running this command:

    git add -A

### Add set or add an origin to the repository.

If the local repository was created locally without being pulled remotely, then you might need to set the remote origin or add an origin. First let’s check if you have remote origins set like this:

    git remote -v

If nothing is set, then set the remote origin like this:

	git remote set-url origin git@github.com:JackSzwergold/Preworn-Main.git

Or add a remote origin like this:

	git remote add origin git@github.com:JackSzwergold/Preworn-Main.git

### Push the local repository to remote repository.

With all that done, push your local git repository commits to the remote repository like this:

    git push origin master

Or even just with `git push` if GIT is version 2.3 or higher:

    git push

### Cloning a repository.

Let‘s assume a repository already exists and you want to clone it locally to work in it. Just do this:

    git clone git@github.com:JackSzwergold/Preworn-Main.git .

That will clone the `Preworn-Main` directory into a new directory named `Preworn-Main` on you local machine. But if you want to change the name of the local repository’s directory name, just clone it with a new name—like `preworn_repo`—instead of `.` like this:

    git clone git@github.com:JackSzwergold/Preworn-Main.git preworn_repo

### Cleaning a local repository of untracked files.

So let’s say your Git repository somehow has files you don’t want to track in it and those files somehow fall out of the scope of a `.gitignore` file. Something that might be caused by accidentally copying files into the repository or somehow a codebase being infected by rogue malware. You can use `git clean` to clean things up for you.

The first command you should run would be this command using the `-n` flag for a dry run:

    git clean -f -d -n

This would instruct `git clean` to clean up files (`-f`) as well as directories (`-d`).

When you run that command as a dry run, be sure to check the list of files and directories sent back to you to make sure it’s definitely cleaning up files you no longer need.

If you are satisfied that the results are correct, run the same command without the dry-run flag like this:

    git clean -f -d

And now, all of those unwanted files and directories should be gone.

### Changing the author of commits throughout a branch.

If you somehow want to change the author of commits in a branch run this Bash command; be sure to change the values to match your own:

	git filter-branch -f --commit-filter '
	  if [ "$GIT_AUTHOR_EMAIL" = "foobar@example.com" ];
	  then
	    GIT_AUTHOR_NAME="Jack Szwergold";
	    GIT_AUTHOR_EMAIL="JackSzwergold@someplace.com";
	    git commit-tree "$@";
	  else
	    git commit-tree "$@";
	  fi' HEAD

### Removing a directory and commits from a branch.

Use this command to remove a directory from all commits in a branch; in this example the directory name is `some_foobar_directory/`:

	git filter-branch -f --tree-filter 'rm -rf some_foobar_directory ' \
		--prune-empty --tag-name-filter cat -- --all

***

*Git - Initing a Repo and Pushing to Origin (c) by Jack Szwergold; written on September 15, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*

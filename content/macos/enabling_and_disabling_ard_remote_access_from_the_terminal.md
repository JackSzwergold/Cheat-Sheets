---
Title: macOS - Enabling and Disabling ARD Remote Access from the Terminal
Description: A cheat sheet for enabling and disabling ARD remote access from the Terminal.
Author: Jack Szwergold
Date: 2015-09-13
Robots: noindex,nofollow
Template: index
---

### Start ARD from the command line.

Sometimes ARD (Apple Remote Desktop) is deactivated, crashed or somehow doesn’t respond on a remote system. If you can get into the machine via SSH remote login, you can run this command from the Terminal to start ARD:

    sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -access -on -clientopts -setvnclegacy -vnclegacy yes -clientopts -setvncpw -vncpw mypasswd -restart -agent -privs -all

### Stop ARD from the command line.

And if you need to stop ARD on a remote machine via SSH remote login, just run this command:

    sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -deactivate -configure -access -off

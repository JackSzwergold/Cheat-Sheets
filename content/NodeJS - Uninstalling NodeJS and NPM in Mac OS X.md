---
Title: NodeJS - Uninstalling NodeJS and NPM in Mac OS X
Description: NodeJS - Uninstalling NodeJS and NPM in Mac OS X
Author: Jack Szwergold
Date: 2015-02-03
Robots: noindex,nofollow
Template: index
---

To uninstall NodeJS and NPM from Mac OS X run the following command:

    sudo rm -rf /usr/local/{lib/node{,/.npm,_modules},bin,share/man}/{npm*,node*,man1/node*}

Then you might need to run these commands to get rid of remaining stuff:

    sudo rm -rf /usr/local/lib/{node,node_modules}
    sudo rm -rf /usr/local/include/{node,node_modules}
    sudo rm -rf ~/{.npm,.node_repl_history}

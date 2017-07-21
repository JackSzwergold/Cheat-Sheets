## Apache - Cordova

By Jack Szwergold

### Cordova related stuff.

#### Installing and upgrading Cordova.

First install Apache Cordova via the command line like this:

	sudo npm install -g cordova

You can check the version of Cordova by running this command:

    cordova -v

If you need to update your install of Cordova, just run this command:

    sudo npm update -g cordova

You can also install a specific version of Cordova using this syntax:

    sudo npm install -g cordova@5.4.1

To check the list of all available versions of Cordova, run this command:

	npm view cordova versions

Check the Cordova requirements to see if they have been met by running this command:

    cordova requirements

#### Basic steps.

With that done, create a directory for your application like this:

	mkdir test

And go into that directory like this:

	cd test

Create the Cordova app in that directory like this:

	cordova create .

Now setup the `browser` platform like this:

	cordova platform add browser

And run the appliction like this:

	cordova run browser

If you want to set a browser other than the default Chrome—or a different port—just pass through options like this:

	cordova run browser -- --target=safari --port=8000

### Ionic related stuff.

Now install Ionic:

	sudo npm install -g ionic

Go into the `test/` directory:

	ionic start test blank

Go into the `test/` directory:

	cd test

Now setup the `browser` platform like this:

	ionic platform add browser

Build the `browser` app:

	ionic build browser

And now run the `browser` app:

	ionic emulate browser

If you want to set a browser other than the default Chrome—or a different port—just pass through options like this:

	ionic emulate browser -- --target=safari --port=8000

### Cleaning out caches and temp files.

Sometimes an upgrade or clean install of EmberJS will mean you need to clean out and reinstall old cruft and dependencies. This is how you go about that.

First, clear out the temp files, Node modules and Bower components:

    rm -rf {node_modules,platforms,plugins}

Next, reinstall the NPM and Bower dependencies:

    npm install && bower install

***

*Apache - Cordova (c) by Jack Szwergold; written on March 11, 2016. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
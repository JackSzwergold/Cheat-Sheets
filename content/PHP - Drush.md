---
Title: PHP - Drush
Description: PHP - Drush
Author: Jack Szwergold
Date: 2016-12-15
Robots: noindex,nofollow
Template: index
---

### Drush related stuff.

* **Drush Git repo**: https://github.com/drush-ops/drush
* **Drush install info**: http://www.drush.org/en/master/install/

Download latest stable release using the code below or browse to github.com/drush-ops/drush/releases:

	php -r "readfile('https://s3.amazonaws.com/files.drush.org/drush.phar');" > drush

Or use this command to get a development release:

    php -r "readfile('https://s3.amazonaws.com/files.drush.org/drush-unstable.phar');" > drush
	
Test your install.

	php drush core-status
	
Make `drush` executable as a command from anywhere. Destination can be anywhere on $PATH.

	chmod +x drush
	sudo mv drush /usr/local/bin
	
Optional. Enrich the bash startup file with completion and aliases.

	drush init

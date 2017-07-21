## PHP - phpMyAdmin Related Items

By Jack Szwergold

### Note regarding PHP 5.1.6 and phpMyAdmin.

Use this version of phpMyAdmin for PHP 5.1.6 installs. But lord help you if you ever have to deal with PHP 5.1.6 nowadays.

	phpMyAdmin-2.11.11.3-english

### Installing phpMyAdmin.

Go into the `/usr/share` directory:

	cd /usr/share

Grab a compressed archive of `phpMyAdmin-4.0.10.11-all-languages.tar.gz` from an official phpMyAdmin source site:

	sudo curl -O -L https://files.phpmyadmin.net/phpMyAdmin/4.0.10.11/phpMyAdmin-4.0.10.11-all-languages.tar.gz

Next, decompress the archive like this:

	sudo tar -xf phpMyAdmin-4.0.10.11-all-languages.tar.gz

And delete the remnant `phpMyAdmin-4.0.10.11-all-languages.tar.gz` archive:

	sudo rm phpMyAdmin-4.0.10.11-all-languages.tar.gz

Move and rename the `phpMyAdmin-4.0.10.11-all-languages phpmyadmin` directory to `phpmyadmin`:

	sudo mv phpMyAdmin-4.0.10.11-all-languages phpmyadmin

And copy the sample `config.sample.inc.php` into a working config named `config.inc.php` like this:

	sudo cp /usr/share/phpmyadmin/config.sample.inc.php /usr/share/phpmyadmin/config.inc.php

Now adjust the ownership and group of the directory to `root` like this:

	sudo chown root:root -R /usr/share/phpmyadmin

### Adding the `blowfish_secret` stuff to the config.

The `blowfish_secret` value is needed for cookie based authentication in phpMyAdmin. To set the value first run this command to generate the `blowfish_secret` value:

    openssl rand -base64 30

Then open up the phpMyAdmin config file:

	sudo nano /usr/share/phpmyadmin/config.inc.php

And set `$cfg['blowfish_secret']` to whatever that `openssl rand` value is.

### Tweaking the phpMyAdmin install.

#### Disable storage and version check warnings.

Disable “The phpMyAdmin configuration storage is not completely configured…” (`PmaNoRelation_DisableWarning`) message and the default version check `VersionCheck` by opening up the phpMyAdmin config file:

	sudo nano /usr/share/phpmyadmin/config.inc.php

And add these configuration items to the bottom of the file:

	// Added 2012-04-17 to disable PMA warning & version check.
	$cfg['PmaNoRelation_DisableWarning'] = true;
	$cfg['VersionCheck'] = false;

#### Disable “Content-Security-Policy” headers.

And disable “Content-Security-Policy” headers so Safari in Mac OS X can properly load images by opening up `Header.class.php`:

	sudo nano /usr/share/phpmyadmin/libraries/Header.class.php

Search for `if (! defined('TESTSUITE')) {` in `public function sendHttpHeaders()` and change that line:

	if (! defined('TESTSUITE')) {

To be disabled like this:

	if (FALSE && ! defined('TESTSUITE')) {

#### Get rid of the PDF export functionality.

If you have no use for the PDF export functionality of phpMyAdmin—and who really needs to export MySQL data as a PDF anyway?—get rid of the PDF export since it uses up lots of RAM and can crash the server; learned that the hard way:

	sudo rm -f /usr/share/phpmyadmin/libraries/plugins/export/PMA_ExportPdf.class.php
	sudo rm -f /usr/share/phpmyadmin/libraries/plugins/export/ExportPdf.class.php

### Setting an Apache config for phpMyAdmin in Ubuntu 12.04.

Now let’s create our own Apache `phpmyadmin.conf` like this:

	sudo nano /etc/apache2/conf.d/phpmyadmin.conf

Here is an example of a basic, secure Apache config for `phpmyadmin`. Note the `Allow from` exceptions; feel free to add any IP address you wish to bypass that secure setup to that list:

	<Directory "/usr/share/phpmyadmin">

	  AuthName "phpMyAdmin Access"
	  AuthType Basic
	  require valid-user
	  AuthUserFile /etc/apache2/htpasswd_phpmyadmin

	  Order Deny,Allow
	  Deny from all
	  Allow from 127.0.0.1 ::1
	  Allow from localhost
	  Allow from 192.168
	  Allow from 10
	  Satisfy Any

	</Directory>

### Setting an Apache config for phpMyAdmin in Ubuntu 14.04.

Now let’s create our own Apache `phpmyadmin.conf` like this:

	sudo nano /etc/apache2/conf-available/phpmyadmin.conf

Here is an example of a basic, secure Apache config for `phpmyadmin`. Note the `Allow from` exceptions; feel free to add any IP address you wish to bypass that secure setup to that list:

	<Directory "/usr/share/phpmyadmin">
	
	  AuthName "phpMyAdmin Access"
	  AuthType Basic
	  require valid-user
	  AuthUserFile /etc/apache2/htpasswd_phpmyadmin
	
	  Require all denied
	  Require ip 127.0.0.1 ::1
	  Require host localhost
	  Require ip 192.168
	  Require ip 10
	
	</Directory>

With that done, be sure to enable the AWStats Apache module like this:

	sudo a2enconf phpmyadmin

***

And if you are using the secure setup, be sure to setup the `/etc/apache2/htpasswd_phpmyadmin` that config refers to like this:

	sudo htpasswd -c /etc/apache2/htpasswd_phpmyadmin phpmyadmin

That command will initiate the process to create an `htpasswd` for a user named `phpmyadmin` in the file named `htpasswd_phpmyadmin`. When prompted, enter whatever password you would like to use.

### Upgrading an existing phpMyAdmin install.

Go into the `/usr/share` directory:

	cd /usr/share

Grab a compressed archive of `phpMyAdmin-4.0.10.11-all-languages.tar.gz` from an official phpMyAdmin source site:

	sudo curl -O -L https://files.phpmyadmin.net/phpMyAdmin/4.0.10.11/phpMyAdmin-4.0.10.11-all-languages.tar.gz

Next, decompress the archive like this:

	sudo tar -xf phpMyAdmin-4.0.10.11-all-languages.tar.gz

And delete the remnant `phpMyAdmin-4.0.10.11-all-languages.tar.gz` archive:

	sudo rm phpMyAdmin-4.0.10.11-all-languages.tar.gz

Copy the current `config.inc.php` from `phpmyadmin` to sample `phpMyAdmin-4.0.10.11-all-languages` like this:

	sudo cp /usr/share/phpmyadmin/config.inc.php /usr/share/phpMyAdmin-4.0.10.11-all-languages/config.inc.php

And delete the existing `phpmyadmin` directory:

	sudo rm -rf /usr/share/phpmyadmin/

Move and rename the `phpMyAdmin-4.0.10.11-all-languages phpmyadmin` directory to `phpmyadmin`:

	sudo mv phpMyAdmin-4.0.10.11-all-languages phpmyadmin

Now adjust the ownership and group of the directory to `root` like this:

	sudo chown root:root -R /usr/share/phpmyadmin

### Dealing with `mysqli` extension errors.

If you attempt to load phpMyAdmin and get the following `mysqli` extension error:

> phpMyAdmin - Error
> 
> The mysqli extension is missing. Please check your PHP configuration. See our documentation for more information.

Just install the PHP MySQL extension like this:

    sudo aptitude install php5-mysql

And restart Apache like this:

    sudo service apache2 restart

Now phpMyAdmin should load without issue.

### Dealing with `mcrypt` extension errors.

If you are able to login to phpMyAdmin but get the following `mcrypt` extension error:

> The mcrypt extension is missing. Please check your PHP configuration.

Just install the PHP MCrypt extension like this:

    sudo aptitude install php5-mcrypt
    
Then enable the module like this:

    sudo php5enmod mcrypt

And restart Apache like this:

    sudo service apache2 restart

***

*PHP - phpMyAdmin Related Items (c) by Jack Szwergold; written on October 17, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
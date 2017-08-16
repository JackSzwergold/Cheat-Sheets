## macOS - MAMP Related Items

By Jack Szwergold

This is how you can setup named virtual hosts on MAMP. First open up the `httpd.conf` file like this:

    nano /Applications/MAMP/conf/apache/httpd.conf

Add this to the bottom of the file. Requests to a `localhost` request would go to one virtual host. Requests to any host with a `*.local` extension go to the other virtual host.

	NameVirtualHost *
	
	<VirtualHost *>
	  DocumentRoot "/Applications/MAMP/htdocs"
	  ServerName localhost
	  ServerAlias localhost
	</VirtualHost>
	
	<VirtualHost *>
	  DocumentRoot "/Applications/MAMP/htdocs/something/else"
	  ServerName *.local
	  ServerAlias *.local
	</VirtualHost>

### Dealing with `DYLD_LIBRARY_PATH` issues connected to MAMP and ImageMagick.

Sometimes binaries compiled with dynamic library requirements choke when being called via `exec()` or `shell_exec()` in MAMP. I first noticed this when having issues with ImageMagick `convert` being called via the `exec()` with errors showing up via `stderr` instead of the standard PHP error log; massive pain in the ass:

    tail -f -n 200 /var/log/system.log

This is due to MAMP setting a `DYLD_LIBRARY_PATH` environment variable. Unsure what is lost by disabling this, but I was able to fix ImageMagick interaction with MAMP by neutering it like this.

First open up the MAMP specific, `envvars` file like this:

    nano /Applications/MAMP/Library/bin/envvars

Then find the line near the bottom that reads like this:

    export DYLD_LIBRARY_PATH

And comment it out like this:

    #export DYLD_LIBRARY_PATH

Finally restart the MAMP servers and all should be good as far as `exec()` and programs that require dynamic libraries like ImageMagick.

### Adjusting the PHP timezone.

Since MAMP is developed in Germany, the default time zone is set to `Europe/Berlin`. To make things easier when debugging things if you are not in Germany is to adjust the timezone like this. First, open up the MAMP specific `php.ini` file like this:

    nano /Applications/MAMP/bin/php/php5.6.10/conf/php.ini

Find this chunk of configuration code:

	; Defines the default timezone used by the date functions
	; Will be changed by MAMP to system timezone
	date.timezone = "Europe/Berlin"

Then adjust it like this; which in this case switches the timezone to `America/New_York`:

	; Defines the default timezone used by the date functions
	; Will be changed by MAMP to system timezone
	; date.timezone = "Europe/Berlin"
	date.timezone = "America/New_York"

With that done, save the file, restart MAMP and the timezone for things like PHP error logs will now be properly set to the `America/New_York` timezone.

### PHP MSSQL under MAMP 3.5.2 or MAMP 4.0.6

Before anything else, make sure your local MAMP install’s binary paths are part of you default PATH. You would do this by adding the following paths to your main PATH in your `~/.bash_profile`:

* **MAMP 3.5.2 Main `bin/` Path**: `/Applications/MAMP/Library/bin`
* **MAMP 3.5.2 PHP `bin/` Path**: `/Applications/MAMP/bin/php/php5.6.10/bin`

#### Compile and Install FreeTDS

We need to install FreeTDS within the installed version of MAMP.

Before anything else, make sure to head to your desktop before anything else happens:

	cd ~/Desktop

Now use Curl to grab the latest stable version of FreeTDS like this:

	curl -O -L ftp://ftp.freetds.org/pub/freetds/stable/freetds-patched.tar.gz

Then decompress the FreeTDS source code archive:

	tar -xf freetds-patched.tar.gz

Now go into the dempressed directory:

	cd freetds-*

And run this `configure` command:

	./configure --prefix=/Applications/MAMP/Library --with-tdsver=7.4 --sysconfdir=/Applications/MAMP/conf/freetds

With that done, run `make`:

	make

And finally install FreeTDS into the installed version of MAMP:

	make install

#### Compile PHP

Now we will download the exact same version of PHP you will be running under MAMP so we can properly compile the MSSQL module. We will not be completely rebuilding PHP from source, but we need the full PHP source coe to build the module against.

Before anything else, make sure to head to your desktop before anything else happens:

	cd ~/Desktop

Now use Curl to grab the PHP source code for the version of PHP you will be using under MAMP like this:

	curl -O -L http://am1.php.net/distributions/php-5.6.10.tar.gz

	tar -xf php-5.6.10.tar.gz

	cd php-5.6.10

And run this `configure` command; we don’t need iconv for what we are doing so we build it like this:

	./configure --without-iconv

If for some reason you need iconv, you can run this `configure` command:

	./configure --with-iconv=/Applications/MAMP/Library/

And finally with that done, run `make`:

	make

#### Compile and Install MSSQL

With PHP now compiled, let’s now go into the MSSQL PHP extension directory:

	cd ext/mssql

We’re going to run `phpize` to prepare the MSSQL PHP extension of compiling:

	phpize

Now run this `configure` command:

	./configure --with-mssql=/Applications/MAMP/Library

And finally with that done, run `make` to compile the MSSQL PHP extension:

	make

***

	cp modules/mssql.so /Applications/MAMP/bin/php/php5.6.27/lib/php/extensions/no-debug-non-zts-20131226/

	echo "extension=mssql.so" >> Applications/MAMP/bin/php/php5.6.27/conf/php.ini

***

*macOS - MAMP Related Items (c) by Jack Szwergold; written on October 7, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
---
Title: macOS - MAMP - Oracle OCI8 Module Items
Description: A cheat sheet for MAMP Oracle OCI8 module items.
Author: Jack Szwergold
Date: 2017-08-17
Robots: noindex,nofollow
Template: index
---

Notes here based on information gleaned from these two excellent blog entries:

* `https://www.thereluctantdeveloper.com/2014/04/adding-the-oracle-extension-oci8-to-mamp-pro-3os-x-109`
* `https://web.archive.org/web/20150811141206/http://caseymorford.com/2014/04/17/installing-oci8-with-mamp-pro/`

***

### PHP Oracle OCI8 under MAMP 6.8 running PHP 8.2.0

Before anything else, make sure your local MAMP install’s binary paths are part of you default PATH. You would do this by adding the following paths to your main PATH in your `~/.bash_profile`:

* **MAMP 6.8 Main `bin/` Path**: `/Applications/MAMP/Library/bin`
* **MAMP 6.8 PHP `bin/` Path**: `/Applications/MAMP/bin/php/php8.2.0/bin`

#### Install the Oracle Instant Client Archives

First, you need to get copies of the “Basic” and “SDK” archives for the Oracle Instant Client directly from Oracle. The archives I used were:

* `instantclient-basic-macos.x64-19.8.0.0.0dbru.zip`
* `instantclient-sdk-macos.x64-19.8.0.0.0dbru.zip`

Once you get those archives, create the directory structure they will live in here:

    sudo mkdir -p /opt/oracle/instantclient/

Now copy—or move—those ZIP archives into that directory and decompress them like this. First, go into the parent Instant Client directory like this:

    cd /opt/oracle/instantclient/

Now unZip the archives like this:

    sudo unzip instantclient-basic-macos.x64-19.8.0.0.0dbru.zip
    sudo unzip instantclient-sdk-macos.x64-19.8.0.0.0dbru.zip

And when that’s done, cleanup by deleting the Zip archives:

    sudo rm *.zip

They should both decompress into the a directory named `instantclient_23_3` which would make the full path:

    /opt/oracle/instantclient/instantclient_23_3/

With that done, go into that directory:

    cd /opt/oracle/instantclient/instantclient_23_3/

And then set these two symbolic links:

    sudo ln -s libclntsh.dylib.12.1 libclntsh.dylib
    sudo ln -s libocci.dylib.12.1 libocci.dylib

#### Compiling and installing the Oracle InstantClient (OCI8) module for PHP.

Now install the OCI 8 version that works with your version of PHP. Here is how it breaks down:

* **PHP 8.2**: `pecl install oci8-3.4.0`
* **PHP 8.1**: `pecl install oci8-3.2.1`
* **PHP 8.0**: `pecl install oci8-3.0.1`
* **PHP 7.x**: `pecl install oci8-2.2.0`
* **PHP 5.2–5.6**: `pecl install oci8-2.0.12`
* **PHP 4.3.9-5.1**: `pecl install oci8-1.4.10`

So in this case, we are installing it for PHP version 8.2 so we will install it using this command:

    pecl install oci8

If you need to force a rebuild, use the `-f` option like this:

    pecl install -f oci8

During the install you might be asked to provide the `ORACLE_HOME` path via a message like this:

    Please provide the path to the ORACLE_HOME directory. Use 'instantclient,/path/to/instant/client/lib' if you're compiling with Oracle Instant Client [autodetect] :

We are not permanently setting `ORACLE_HOME` so just use this as the path:

    instantclient,/opt/oracle/instantclient/instantclient_23_3

After PECL does it’s thing, the `oci8.so` should be compiled and ready to go.

#### Installing the OCI8 Module

[Now adjust the PHP config file (`php.ini`) like this to get PHP to recognize it:

    sh -c "printf '\n[OCI8]\nextension=oci8.so\n' >> /Applications/MAMP/bin/php/php8.2.0/conf/php.ini"

Now start MAMP again and check the output of the PHP info page and `oci8.so` should be clearly listed there under installed components.]()

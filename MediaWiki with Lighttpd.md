# How To Set Up the Latest MediaWiki with Lighttpd on Ubuntu 14.04

By Jack Szwergold

### Introduction

MediaWiki is a popular open source wiki platform that can be used for public or internal collaborative content publishing. MediaWiki is used for many of the most popular wikis on the internet including Wikipedia, the site that the project was originally designed to serve.

In this guide, we will be setting up the latest version of MediaWiki on an Ubuntu 14.04 server. We will use the lighttpd web server to make the actual content available, php-fpm to handle dynamic processing, and mysql to store our wiki's data.

## Prerequisites

To complete this guide, you should have access to a clean Ubuntu 14.04 server instance. On this system, you should have a non-root user configured with sudo privileges for administrative tasks. You can learn how to set this up by following our [Ubuntu 14.04 initial server setup guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04).

When you are ready to continue, log into your server with your sudo user and get started below.

## Step One — Install the Server Components

First, login to the system you wish to install MediaWiki onto and update your repository lists like this:

    sudo apt-get update

Next, make sure you have **curl** installed by running this command to check the installed version:

    curl -V

If all went well, the output of that command should look something like this:

	curl 7.35.0 (x86_64-pc-linux-gnu) libcurl/7.35.0 OpenSSL/1.0.1f zlib/1.2.8 libidn/1.28 librtmp/2.3
	Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smtp smtps telnet tftp 
	Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP 

If the response is something like:

    curl: command not found

Install **curl** by running this command:

    sudo apt-get install -y --assume-yes curl

Once that’s done, you’re good to move onto the next step.

## Step Two — Configure MySQL and Create Credentials for MediaWiki

Now that we know that the system you are working on is properly setup, let’s get MySQL installed like this:

    sudo apt-get install -y --assume-yes mysql-server

At some point during initial MySQL install, the process will prompt you to choose a **root** password. Type in whatever **root** password you feel comfortable with and hit return to move on.

Now, let’s do some basic MySQL hardening by running this command:

    mysql_secure_installation

You’ll be prompted for the MySQL **root** password that was just created; enter that and hit return. Answer the remaining prompts as follows:

* **Change the root password?** Select **N** for “No.”
* **Remove anonymous users?** Select **Y** for “Yes.”
* **Disallow root login remotely?** Select **Y** for “Yes.”
* **Remove test database and access to it?** Select **Y** for “Yes.”
* **Reload privilege tables now?** Select **Y** for “Yes.”

With that all done, let’s login into your server’s newly installed MySQL instance with this command; note you will be prompted for your **root** password:

    mysql -u root -p

And once you’re at the MySQL command prompt, let’s create a MediaWiki specific database—named **mediawiki**—like this:

    CREATE DATABASE `mediawiki`;

With that done, let’s now create a MediaWiki specific user—with exclusive rights to that **mediawiki** database—by running this command; please be sure change the **[password]** text in this example to the actual password you would be using for the install:

    CREATE USER 'mediawiki'@'localhost' IDENTIFIED BY '[password]';

Now grant that **mediawiki** user access rights to the **mediawiki** database by running the following two MySQL **GRANT** commands:

    GRANT USAGE ON `mediawiki`.* TO 'mediawiki'@'localhost' IDENTIFIED BY 'mediawiki';
    GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, EVENT, TRIGGER ON `mediawiki`.* TO 'mediawiki'@'localhost';

As a final step, we need to set the user’s password again like this; please be sure change the **[password]** text in this example to the actual password you would be using for the install:

    SET PASSWORD FOR 'mediawiki'@'localhost' = PASSWORD('[password]');

And with that done, we need to run this command to get MySQL to reload—and recognize—the newly adjusted grant table settings:

    FLUSH PRIVILEGES;

Once that’s done you can exit MySQL by simply typing `exit;` and hitting return which will get you back to your server’s terminal shell where we can move onto the next step: Getting Lighttpd installed and configured.

## Step Three — Configure Lighttpd and PHP-FPM

We are going to be using Lighttpd and PHP-FPM as our web server for MediaWiki in this tutorial, so let’s go ahead and install it like this.

First, run this command to install Lighttpd itself:

    sudo apt-get install -y --assume-yes lighttpd

With that done you can test the install to see if Lighttpd is up and running by opening up a web browser and pointing it to the domain name or IP of your server like this; change **domain_name_or_IP** to match the domain name or IP address of the server you are working on:

    http://domain_name_or_IP

If all went well you’ll see a default placeholder Lighttpd web page. But if somehow the Lighttpd service does not start, you can force it to come up by running this command:

    sudo service lighttpd start

***

**Note:** While Lighttpd should normally be set as a startup service during the install process, one might need to run this `update-rc.d` command to get the defaults set so Lighttpd comes back up on reboot/cold boot:

    sudo update-rc.d lighttpd defaults

***

Now that Lighttpd is setup and running, let’s install the PHP related items like this:

    sudo apt-get install -y --assume-yes php5-cgi php5-mysql

And once those are installed, run this command to ensure the PHP FastCGI modules are enabled:

    sudo lighty-enable-mod fastcgi fastcgi-php

With that done, let’s restart Lighttpd to get those modules loaded:

    sudo service lighttpd restart

And now, let’s test the PHP install by creating a simple test file like this; I’m using **nano** for a text editor in this example, but feel free to use whatever text editor you prefer:

    sudo nano /var/www/test.php

And the contents of that file should be something like this:

    <?php

    phpinfo();

    ?>

Save that newly created *test.php*, exit your text editor, and now load that file it in your web browser like this; change **domain_name_or_IP** to match the domain name or IP address of the server you are working on:

    http://domain_name_or_IP/test.php

If everything goes as planned, that page should load with the standard PHP info that reflects all current PHP—and related—server settings on that server. To make sure that everything is working as we want for our purposes, be sure to check under the **Server API** and make sure it’s set to **CGI/FastCGI**.

With that done, let’s move onto getting the MediaWiki itself installed.

## Step Four — Install MediaWiki

With MySQL and Lighttpd (with PHP) installed and configured, we’re now read to install MediaWiki itself.

First, let’s prep the web root of the server by removing the—what are now—unneeded default **index.lighttpd.html** and **test.php** files from the web root by running this **rm** command:

    sudo rm /var/www/index.lighttpd.html /var/www/test.php

With that done, let’s get a copy of the latest version of MediaWiki onto the server by running this **curl** command:

    curl -O https://releases.wikimedia.org/mediawiki/1.27/mediawiki-1.27.0.tar.gz

Once it’s been fully downloaded, let’s decompress the Tar archive with this command:

    tar -xf mediawiki-*.tar.gz

And once the archive is decompressed let’s copy the contents of that directory right into the web root like this:

    sudo mv mediawiki-*/* /var/www/

And finally, let’s change user and group ownership of those those files and directories to **www-data** by running this **chown** command:

    sudo chown -R www-data:www-data /var/www/*

Now let’s check if MediaWiki is running by opening up a web browser and pointing it to the domain name or IP of your server like this; change **domain_name_or_IP** to match the domain name or IP address of the server you are working on:

    http://domain_name_or_IP/

And if everything has gone well, you should see a page that has the MediaWiki logo and version number on it and says:

> LocalSettings.php not found.

> Please set up the wiki first.

Just click that **set up the wiki** link to begin the setup procedure.

First, select the desired language settings—for the user and the wiki itself—and hit continue.

Next up will be a page that titled “Welcome to MediaWiki!” which does some basic system environment checks. If all went well, you should see the following text in green near the middle of the page:

> The environment has been checked. You can install MediaWiki.

If that’s all good, just click **Continue** to move onto the next step; if something went wrong follow the instructions on that page to see what went wrong and what you can do to fix it. Once that’s cleared up, move onto the next step.

Now you will be on a page where you can enter info to get your install of MediWiki connected to a database. We will be using the MySQL credentials and details that were setup previously as follows:

* The database host should be: **localhost**
* The database name should be: **mediawiki**
* The database table prefix should be left blank.
* The database username and password should be set to what you set them to previously.

When those items are set, just hit **Continue** and move onto the database settings page.

On the database settings page, the **Storage Engine** and **Database Character Set** values should be fine at their defaults of **InnoDB** and **Binary** so just hit **Continue** and move onto the Wiki specific details.

Here you would choose a name of the Wiki as well as set basic wiki administrator specific account info; just set it up whichever way you please.

Once those values are set, **Continue** and you can set and adjust other, deeper Wiki specific options, but you can just click **Continue** and accept the defaults for now; everything can be changed later on anyway.

The next, penultimate page will state:

> By pressing "Continue →", you will begin the installation of MediaWiki. If you still want to make changes, press "← Back".

Just go ahead and click **Continue** and if all proceeds as expected, you will reach the final “Complete!” screen and a file named **LocalSettings.php** will be downloaded to your local system via your web browser.

You now need to get that **LocalSettings.php** file from your local machine in some way and install it right inside your server’s MediaWiki install root.

For example—assuming the **LocalSettings.php** was downloaded into your user’s **/Downloads/** directory—you could copy it from there to your server user’s home directory by running this **scp** command; be sure to change **[username]** and **domain_name_or_IP** to match your specific user name and server address values:

    scp ~/Downloads/LocalSettings.php [username]@[domain_name_or_IP]:.

Once that has been copied over, login to your server and just move that **LocalSettings.php** file over to the your installed MediaWiki root by running this **mv** command:

    sudo mv ~/LocalSettings.php /var/www/LocalSettings.php

And make sure to change the file’s owner and group to **www-data**  by running this **chown** command:

    sudo chown www-data:www-data /var/www/LocalSettings.php

And finally, set the permissions of the file to **600** (aka: owner read and write only) to secure the config on the server:

    sudo chown 600 /var/www/LocalSettings.php

Once that’s done, your install of MediaWiki should be complete! You can check this by opening up a web browser and pointing it to the domain name or IP of your server like this; change **domain_name_or_IP** to match the domain name or IP address of the server you are working on:

    http://domain_name_or_IP/

## Conclusion

With all of that done, you should now have MediaWiki fully installed and configured on your system.

***

*MediaWiki with Lighttpd (c) by Jack Szwergold; written on August 1, 2016. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
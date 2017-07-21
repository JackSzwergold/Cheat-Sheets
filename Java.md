## Java

By Jack Szwergold

### Installing Java via the WebUpd8 PPA repository.

#### Adding the WebUpd8 PPA repository to the system.

First install `python-software-properties` like this:

	sudo aptitude install python-software-properties

Next add the WebUpd8 PPA repository to the system like this:

	sudo add-apt-repository ppa:webupd8team/java

With that done, run `aptitude update` to get the new repository picked up like this:

	sudo aptitude update

### Install various versions of Java via the WebUpd8 PPA.

#### Install Java 6.

Now install Java (version 6) via `aptitude` like this:

    sudo aptitude install oracle-java6-installer

Check the version number like this:

    java -version

And the output should be something like this:

	Java(TM) SE Runtime Environment (build 1.6.0_45-b06)
	Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)

#### Install Java 7.

Now install Java (version 7) via `aptitude` like this:

    sudo aptitude install oracle-java7-installer

Check the version number like this:

    java -version

And the output should be something like this:

	java version "1.7.0_80"
	Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
	Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)

#### Install Java 8.

Now install Java (version 8) via `aptitude` like this:

    sudo aptitude install oracle-java8-installer

Check the version number like this:

    java -version

And the output should be something like this:

	java version "1.8.0_60"
	Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
	Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)

### Sundry Java items.

#### Add a `JAVA_HOME` value to the system `/etc/environment` file.

Lots of apps look for a `JAVA_HOME` variable on a system. To make your life easier, set the `JAVA_HOME` after you install Java so you don’t have to hunt for custom settings for each application.

You can do this by opening up the main `/etc/environment` file on the system like this:

    sudo nano /etc/environment

And add a `JAVA_HOME` assignment to the bottom of the file like this; adjust the `java-6-oracle` value to match whatever Java version you have installed:

	JAVA_HOME=/usr/lib/jvm/java-6-oracle/jre

#### Detect the version of Java a class was compiled under

If you have ever need to check what version of Java compiled a Java class, just run the following command. Be sure to change `/path/to/the/java.class` to match the full path of the actual Java class you want check:

    javap -verbose /path/to/the/java.class | grep "public class javaversion" -A 2

The output of that command should be something like this:

	public class javaversion
	minor version: 3
	major version: 45

Cross reference the “major version” value with the following list:

	Java 1.0 uses major version 45
	Java 1.1 uses major version 45
	Java 1.2 uses major version 46
	Java 1.3 uses major version 47
	Java 1.4 uses major version 48
	Java 5 uses major version 49
	Java 6 uses major version 50
	Java 7 uses major version 51

And using that table above, we can see that a Java class with a major version of 45 translates to Java 1.0/1.1.

### Removing older Java items.

If you are somehow working on a system where Java was installed via the default Ubuntu “partner” repository or via the `oab-java.sh` method, here is how you can clean out these installs so they don’t muck things up with the WebUpd8 PPA repository install of Java.

#### Get rid of the default Ubuntu “partner” repository installed version of Java.

Just run this `sudo aptitude purge` command to get rid of any Java stuff installed via the default Ubuntu “partner” repository:

	sudo aptitude purge java-common libcommons-collections3-java libcommons-dbcp-java libcommons-pool-java libecj-java libservlet2.5-java libtomcat6-java oracle-java6-installer sun-java6-bin sun-java6-jre

#### Removing the `oab-java.sh` Java install items.

If you somehow installed Java previously via the `oab-java.sh` script, you can get rid of that stuff like this.

First, check what is in the `sources.list` directory:

	ls -la /etc/apt/sources.list.d/

Next, get rid of the previously installed `oab-java.sh` Java packages:

	sudo aptitude purge sun-java6-jdk sun-java6-fonts sun-java6-source

Now, remove the `oab-java.sh` PPA (Personal Package Archive) stuff:

    sudo rm /etc/apt/sources.list.d/oab.list

Finally, remove the old `oab-java.sh` items from `/var/local/`.

***

*Java (c) by Jack Szwergold; written on September 21, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
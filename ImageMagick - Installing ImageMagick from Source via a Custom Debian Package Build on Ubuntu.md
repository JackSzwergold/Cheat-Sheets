## ImageMagick - Installing ImageMagick from Source via a Custom Debian Package Build on Ubuntu

By Jack Szwergold

### Get rid of the default Ubuntu repository installed version of ImageMagick.

Just run this `sudo aptitude purge` command to get rid of any ImageMagick stuff installed via the default Ubuntu repository:

    sudo aptitude purge imagemagick

### Install the ImageMagick source code prerequisites and dependencies.

Install the basics for the build:

    sudo aptitude install build-essential checkinstall libx11-dev libxext-dev zlib1g-dev libpng12-dev libjpeg-dev libfreetype6-dev libxml2-dev

Build the dependencies for the build:

    sudo aptitude build-dep imagemagick

### Install ImageMagick from source using `checkinstall`.

First grab a compressed archive from an official ImageMagick source site:

	curl -O -L http://www.imagemagick.org/download/ImageMagick.tar.gz
	
Next, decompress the archive like this:

	tar -xf ImageMagick.tar.gz
	
Now go into the decompressed directory:

	cd ImageMagick-*
	
Run this `configure` command:

	./configure
	
Finally compile and install it by running `checkinstall`:

	sudo checkinstall

Just punch through the default options presented and any questions that might pop up during the Debian package build.

When all done you will get a message like this on the screen which means ImageMagick is installed:

	**********************************************************************
	
	 Done. The new package has been installed and saved to
	
	 /home/sysop/ImageMagick-6.9.3-0/imagemagick-6.9.3_0-1_amd64.deb
	
	 You can remove it from your system anytime using: 
	
	      dpkg -r imagemagick-6.9.3
	
	**********************************************************************

After that is done, you may need to configure the dynamic linker run-time bindings:

	sudo ldconfig /usr/local/lib

Finally, check the version number like this:

    convert -version

And the output should look something like this:

	Version: ImageMagick 6.9.3-0 Q16 x86_64 2016-01-17 http://www.imagemagick.org
	Copyright: Copyright (C) 1999-2016 ImageMagick Studio LLC
	License: http://www.imagemagick.org/script/license.php
	Features: Cipher DPC OpenMP 
	Delegates (built-in): bzlib djvu fontconfig freetype jbig jng jpeg lcms lqr lzma openexr pangocairo png tiff wmf x xml zlib

### Uninstall ImageMagick from source using `dpkg -r`.

Go into the decompressed directory where the initial ImageMagick install happened:

	cd ImageMagick-*

And run this `dpkg -r` command to uninstall it:

    sudo dpkg -r imagemagick-6.9.3

***

*ImageMagick - Installing ImageMagick from Source via a Custom Debian Package Build on Ubuntu (c) by Jack Szwergold; written on September 21, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
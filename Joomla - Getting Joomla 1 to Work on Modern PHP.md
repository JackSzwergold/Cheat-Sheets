## Joomla - Getting Joomla 1 to Work on Modern PHP

By Jack Szwergold

If you somehow have the misfortune to deal with keeping an ancient Joomla 1.0 install up and running on a modern server running Apache with PHP 5.3 or higher, this is what you need to do.

### First, edit `/includes/Cache/Lite/Function.php`.

Open up the `/includes/Cache/Lite/Function.php`v

    nano /var/www/joomla_1_x/includes/Cache/Lite/Function.php

Find this chunk of code:

	$arguments = func_get_args();

And replace it with this code:

	$arguments = func_get_args();
	$numargs = func_num_args();
	for($i=1; $i < $numargs; $i++){
	  $arguments[$i] = &$arguments[$i];
	}

### Next, edit `/includes/vcard.class.php`.

Open up the `/includes/vcard.class.php` file like this:

    sudo nano /var/www/joomla_1_x/includes/vcard.class.php

Now wrap the code around lines 38 to 77 in this conditional:

	if(!function_exists('quoted_printable_encode'))
	{
	  /* line 38 to 77 */
	}

***

*Joomla - Getting Joomla 1 to Work on Modern PHP (c) by Jack Szwergold; written on October 5, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
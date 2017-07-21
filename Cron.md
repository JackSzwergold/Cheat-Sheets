## Cron

By Jack Szwergold

#### Start, stop and control the Cron service on an Ubuntu/Debian system.

	sudo service cron start
	sudo service cron stop
	sudo service cron restart

#### Sundry Cron items

To prevent cron jobs from triggering e-mails, add this to the end of the command:

    */30 * * * * /path/to/command/goes/here/example.sh >/dev/null 2>&1

#### Redirecting Cron status messages to `/var/log/cron.log` on a CentOS/RedHat system.

Open up `/etc/syslog.conf` and uncomment the line starting with `cron.*` to get Cron status messages sent to `/var/log/cron.log`.

	sudo nano /etc/syslog.conf

Now restart the `rsyslog` service:

	sudo /etc/init.d/sysklogd restart

Now restart the Cron service:

	sudo /etc/init.d/cron restart

#### Redirecting Cron status messages to `/var/log/cron.log` on an Ubuntu/Debian system.

Open up `/etc/rsyslog.d/50-default.conf` and uncomment the line starting with `cron.*` to get Cron status messages sent to `/var/log/cron.log`:

	sudo nano /etc/rsyslog.d/50-default.conf

Now restart the `rsyslog` service:

	sudo service rsyslog restart

Now restart the Cron service:

	sudo service cron restart

***

*Cron (c) by Jack Szwergold; written on September 19, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
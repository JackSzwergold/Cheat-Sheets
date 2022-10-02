---
Title: PHP - Microsoft SQLSRV Module Items
Description: PHP - Microsoft SQLSRV Module Items
Author: Jack Szwergold
Date: 2019-01-25
Robots: noindex,nofollow
Template: index
---

### PHP Microsoft SQLSRV via Yum repos.

Simple way if the setup is using the Microsoft repos. First, the install the system ODBC stuff:

	sudo su
	curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-tools.repo
	sudo ACCEPT_EULA=Y yum install msodbcsql17 mssql-tools
	exit

Then install the Unix ODBC development stuff:

	sudo yum install unixODBC-devel

Now install the PHP SQLSRV module items via Yum:

	sudo ACCEPT_EULA=Y yum install php-sqlsrv

### PHP Microsoft SQLSRV via Yum repos and PECL.

First, the install the system ODBC stuff:

	sudo su
	curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-tools.repo
	sudo ACCEPT_EULA=Y yum install msodbcsql17 mssql-tools
	exit

For some reason it’s recomended that the UTF16 stuff get removed:

	sudo yum remove unixODBC-utf16 unixODBC-utf16-devel

Add the MSSQL tool stuff to the user environment paths:

	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

Re-source the `.bashrc`:

	source ~/.bashrc

Then install the Unix ODBC development stuff:

	sudo yum install unixODBC-devel

Now install the PHP SQLSRV module items via PECL:

	sudo pecl install sqlsrv pdo_sqlsrv

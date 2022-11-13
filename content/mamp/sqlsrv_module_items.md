---
Title: macOS - MAMP - SQLSRV Module Items
Description: A cheat sheet for MAMP SQLSRV module items.
Author: Jack Szwergold
Date: 2019-01-25
Robots: noindex,nofollow
Template: index
---

### PHP SQLSRV under MAMP running PHP 7.x and Above

Before anything else, make sure your local MAMP install’s binary paths are part of you default PATH. You would do this by adding the following paths to your main PATH in your `~/.bash_profile`:

* **MAMP 5.2 Main `bin/` Path**: `/Applications/MAMP/Library/bin`
* **MAMP 5.2 PHP `bin/` Path**: `/Applications/MAMP/bin/php/php7.2.10/bin`

#### Compile and Install the SQLSRV Stuff

Do this to install the core ODBC stuff on macOS:

    brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
    brew update
    brew install msodbcsql17 mssql-tools

Now install the actual PHP modules via PECL:

    pecl install sqlsrv pdo_sqlsrv

And finally add these lines to your PHP config file (`php.ini`):

    ; Enable 'Microsoft Drivers for PHP for SQL Server' extension module
    extension = sqlsrv.so
    extension = pdo_sqlsrv.so
    
    ; Configuration
    
    ;sqlsrv.WarningsReturnAsErrors = 1
    ;sqlsrv.LogSeverity = 0
    ;sqlsrv.LogSubsystems = 0
    ;sqlsrv.ClientBufferMaxKBSize = 10240
    
    ;pdo_sqlsrv.log_severity = 0
    ;pdo_sqlsrv.client_buffer_max_kb_size = 10240

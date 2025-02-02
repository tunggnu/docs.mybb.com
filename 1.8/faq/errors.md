---
layout: page
title:  "Error Messages"
categories: [faq]
description: "Common MyBB error messages and how to fix them."

redirect_from:
- /Help-Common_Error_Messages.html
---

## Introduction
When using MyBB, you may encounter an Internal MyBB Error, an SQL error, or see a permissions error of some sort. These can limit what you can do with your forum, or may stop you using it altogether. This page will show you some of the common errors people encounter, along with how to fix the problem.

### Enabling Error Logs

MyBB, depending on the board's settings, may not always show error details. To enable error logging, in the Admin CP's _Configuration → Settings → Server and Optimization Options_:
1. set **Use Error Handling** to _Yes_ to show additional settings,
1. set **Error Logging Medium** to _Log errors_,
1. set **Error Logging Location** to e.g. `./error.log`, a location that cannot be accessed through the web (see the [Security Guide](/1.8/administration/security/protection/#apply-server-specific-directives) for server configuration instructions).

If the Admin CP cannot be accessed, these settings can be overridden manually by adding the following lines at the end of the `inc/settings.php` file:
```php
// manual overrides
$settings['errorlogmedium'] = 'log';
$settings['errortypemedium'] = 'both';
$settings['errorloglocation'] = './error.log';
```
_Note: Manual changes are temporary and will be lost when modifying settings using the Admin CP._

The configured file will contain information useful in identifying the cause.

## Error Types
### Internal MyBB Errors
- #### MyBB Error (41)
  **Error Type:** *MyBB Error (41)*

  **Error Message:** Your board has not yet been installed and configured. Please do so before attempting to browse it.

  **Information:** This occurs when the file `inc/config.php` is incorrectly configured. If you have not already installed your forum, you need to go to `install/index.php` and run through the installation process, including entering your database details. If you have already installed your forum, and you have data in your database, then you will need to open `inc/config.php`, use this sample `inc/config.php` text, and edit in your database information.

- #### MyBB Error (43)
  **Error Type:** *MyBB Error (43)*

  **Error Message:** The install directory (`install/`) still exists on your server and is not locked. To access MyBB please either remove this directory or create an empty file in it called `lock`.

  **Information:** This occurs when your `install` folder is still present, or has not been locked. To solve this error, either completely delete the `install` folder, or create an empty file called `lock` inside the folder. MyBB requires you to remove or lock this folder so that nobody can run the installation or upgrade script and corrupt or erase your database with it.

- #### MyBB Error (42)
  **Error Type:** *MyBB Error (42)*

  **Error Message:** Your board has not yet been upgraded. Please do so before attempting to browse it.

  **Information:** This occurs when you are attempting to do a major upgrade (e.g. 1.6.x to 1.8.x) and do not run the upgrade script after uploading the new files. To fix this, go to `install/upgrade.php` and choose your old version from the list and follow the directions given by the upgrader..

- #### MyBB Error (44)
  **Error Type:** *MyBB Error (44)*

  **Error Message:** MyBB was unable to load the SQL extension. Please contact the MyBB Group for support. MyBB Website

  **Information:** This occurs when the database type is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entry for `$config['database']['type']`. A common issue is having `mysql` instead of `mysqli`, or having `mysqli` instead of `mysql`. If you do not know what needs to be here, contact your host provider.

### SQL Errors

- #### MySQL Error 0 -
  **Database:** *MySQL*

  **SQL Error:** `0 -`

  **Query:** `[READ] Unable to select database`

  **Information:** This occurs when you are using MySQL and the database name or database username is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entries for `$config['database']['database']` and `$config['database']['username']`. If you do not know what needs to be here, contact your host provider.

- #### MySQL Error 2005
  **Database:** *MySQL*

  **SQL Error:** `2005 - Unknown MySQL server host 'HOSTNAME' (11004)`

  **Query:** `[READ] Unable to connect to MySQL server`

  **Information:** This occurs when you are using MySQL and the database host name is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entry for `$config['database']['hostname']`. If you do not know what needs to be here, contact your host provider.

- #### MySQL Error 1045
  **Database:** *MySQL*

  **SQL Error:** `1045 - Access denied for user 'USERNAME'@'HOSTNAME' (using password: YES)`

  **Query:** `[READ] Unable to connect to MySQL server`

  **Information:** This occurs when you are using MySQL and the database password is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entry for `$config['database']['password']`. If you do not know what needs to be here, contact your host provider.

- #### MySQL Error 1146
  **Database:** *MySQL*

  **SQL Error:** `1146 - Table 'forum_mybb14x.test_datacache' doesn't exist`

  **Query:** `SELECT title,cache FROM test_datacache`

  **Information:** This occurs when you are using MySQL and the database table prefix is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entry for `$config['database']['table_prefix']`. If you do not know what needs to be here, contact your host provider.

- #### MySQL Error 2013
  **Database:** *MySQL*

  **SQL Error:** `2013 - Lost connection to MySQL server during query`

  **Query:** `SELECT title,cache FROM mybb_datacache`

  **Information:** *This is an issue with the connection to your database, your host will have to solve this issue for you.

- #### SQLite Error 1
  **Database:** *SQLite*

  **SQL Error:** `1 - no such table: test_datacache`

  **Query:** `*SELECT title,cache FROM test_datacache`

  **Information:** This occurs when you are using SQLite and the database table prefix is incorrect in `inc/config.php`. To fix this, open `inc/config.php` and check the entry for `$config['database']['table_prefix']`. If you do not know what needs to be here, contact your host provider. This error also shows when the database path is incorrect in `$config['database']['database']`. To fix this, make sure the path points to where your database is stored.

- #### MySQL Error 2002
  **Database:** *MySQL*

  **SQL Error:** `2002 - Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock'`

  **Query:** `[READ] Unable to connect to MySQL server`

  **Information:** This occurs when your MySQL server is down or experiencing an issue and MyBB cannot connect to it. You will have to contact your host with this error message.

- #### MySQL Error 1054
  **Database:** *MySQL*

  **SQL Error:** `1054 - Unknown column 'loginattempts' in 'field list'`

  **Query:** `SELECT loginattempts FROM mybb_users WHERE LOWER(username)='username' LIMIT 1`

  **Information:** Note: this fix does not apply to all *unknown column* errors, it is specifically for the above specific query (with a different username/table prefix etc of course). Please do not suggest this fix when someone has any other *unknown column* error.

  This occurs when you are using MySQL and you attempt to upgrade from 1.4.3 to 1.4.4, after you upload all the new files, but do not run the upgrade script. In 1.4.4, a new column, `loginattempts`, was added to the users table for a new security feature. The error shows as the 1.4.4 `member.php` file is trying to find the `loginattempts` column in a 1.4.3 database, where it doesn't exist. To fix this issue, go to `install/upgrade.php` and choose 1.4.2/1.4.3 from the list.

## Other Error Messages
- #### *The installer is currently locked*
  *The installer is currently locked, please remove `lock` from the install directory to continue*

  **Information**: This happens when you try to run the installation or upgrade script, but your `install/` folder is locked. Simply remove the `lock` file from the `install/` folder to continue.

- #### *403 Forbidden*
  *You don't have permission to access `admin/index.php` on this server.*

  **Information:** This is nearly always a problem with *mod_security*. *mod_security* is an Apache extension that is used for security. In MyBB 1.4, *mod_security* treats the way the URLs are set out in the ACP as a security threat, so it will deny you access.

  You should contact your host provider and ask them to whitelist your domain or hosting account against *mod_security* - basically so that it doesn't affect you anymore.

- #### *404 Not Found* for Internal Links
  **Information:** When following internal links (forums, threads, etc.) with [Search Engine Friendly URLs](/1.8/administration/configuring-search-engine-friendly-URLs/) enabled, a HTTP 404 error may appear when the URL redirects are not properly configured. Make sure the appropriate file or code fragment is used, or disable the _Enable search engine friendly URLs_ setting.

- #### Catchable Fatal Error (4096)
  This error occurs when you upgrade your forum from MyBB 1.6.4 or older to MyBB 1.6.5 or newer, or when you install a theme that was not updated properly. The *member_register* template has to be updated.

  1. Go to **Admin CP > Templates > Your Theme's Template Set > Member Templates > member_register**.

  2. Find:

      `{$captcha}`

  3. Replace it with:

      `{$hiddencaptcha}`

- #### _Parser output validation failed_
  This error may be logged when the MyCode parser produces invalid XHTML code. Validation errors can also result in the **content of some posts and messages appearing empty**.

  Invalid HTML is usually added to post content by customizations and extensions, like:
  - MyCode _Replacement_ values of custom MyCodes
  - theme templates used for parsing MyCode
  - plugins related to post parsing
  - user groups' _Username Style_

  ##### Identifying the Cause

  To identify the problematic MyCode:
  - copy the content of the broken message to a new thread, and remove custom MyCodes or other non-default features (embeds, mentions, etc.), or
  - temporarily deactivate all or suspected plugins (you can use the _Configuration → Setings → Disable All Plugins_ setting)

  and see if the errors continue to occur.

  If the errors are related to extensions, we recommend checking for new versions or contacting their authors. You can also ask for help in [MyBB Support]({{ site.mainsite_url }}/support/) channels.

  ##### Correcting Validation Issues

  Use the technical details of the error saved in the [error log file](/1.8/faq/errors/#enabling-error-logs) to identify the cause. These include the original message, the resulting output, and detailed validation errors related to the HTML structure.

  You can use the [Parser Validation Debug](https://github.com/dvz/mybb-tools/raw/main/_parser-debug.php) tool to inspect validation details of selected posts on the forum, or decode raw errors that were logged in the past (see [usage instructions](https://github.com/dvz/mybb-tools#usage)).

  ##### Disabling Validation
  Parser output validation is a security feature added in MyBB 1.8.27, and can be temporarily disabled by modifying the [`inc/class_parser.php`](https://github.com/mybb/mybb/blob/mybb_1827/inc/class_parser.php#L122) file, and changing the `$output_validation_policy` value to `VALIDATION_DISABLE`:
  ```php
  public $output_validation_policy = VALIDATION_DISABLE;
  ```
  Once the underlying problems are resolved, restore the original value.

### Useful links
- [Upgrade MyBB](/1.8/install/upgrade/)
- [Fixing database errors](/1.8/faq/database-errors/)
- [Support]({{ site.mainsite_url }}/support/)

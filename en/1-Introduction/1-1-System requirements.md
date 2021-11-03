# System requirements

isEngine can work with Apache and Nginx. It is also possible to work with other web servers, but they may require additional configuration.

## Minimum server requirements

* PHP version 5.6.40 or higher

For PHP, the following extensions must be installed and enabled (usually already enabled by default):

* curl,
* date,
* json,
* mbstring,
* pcre,
* session,
* zip

## Recommended server requirements

* PHP version 7.4 and higher

IsEngine was originally developed with support for version 5.6. This was done for easy migration of sites and web applications from old PHP versions to new ones. Especially when several sites are hosted on the same hosting. At the moment, most hosting providers no longer support PHP versions lower than 7.0. However, we are still trying to provide support for older PHP versions.

Besides PHP extensions for minimum requirements, it is recommended to install and enable the following extensions:

* SimpleXML,
* intl,
* fileinfo,
* gd or imagick,
* PDO, mysqli or sqlite3

All of these extensions are optional for the system, but are required for some modules to work.

## Сomposer

The Composer package manager is optional for the system to work.

However, for the convenience of installing and updating libraries, modules and components, we recommend installing it on the server:

> [https://github.com/composer/composer/releases](https://github.com/composer/composer/releases)

## Database requirements

isEngine works with databases through intermediate classes called drivers.

At the moment, the default database type is a database built in the form of local files located in a separate folder on the server.

By the time of release, it is planned to release drivers for working with MySQL, PostgreSQL, SQLite databases.

You can also help with writing your own drivers for these and other databases.

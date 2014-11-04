# Homebrew (E)Nginx MySQL PHP Installer

Native way to setup web environment for Mac.

![brew-emp](brew-emp.png)

## Overview

This script will install and setup **Nginx** + **MySQL** + **PHP** through **Homebrew**.

## Requirements

* [Homebrew](http://mxcl.github.com/homebrew/)
* Snow Leopard, Lion, Mountain Lion. Untested everywhere else
 
## Installation
`curl -L https://raw.github.com/mrded/brew-emp/master/install.sh | bash`

## Usage
`brew-emp [start | stop | restart]`

## Creating VirtualHost
For example let's create virtual host for Drupal 'mysite'.

Copy Drupal preset:
`cp /usr/local/etc/nginx/sites-available/drupal.local /usr/local/etc/nginx/sites-available/mysite.local`
    
Change name and path to site: 
`vim /usr/local/etc/nginx/sites-available/mysite.local`
    
    
    server {
      listen       80;
      server_name  mysite.local;
      root /Users/mrded/Sites/mysite;

      access_log /usr/local/var/log/nginx/mysite.access.log;
      error_log  /usr/local/var/log/nginx/mysite.error.log;

      include /usr/local/etc/nginx/common/drupal;
    }

Enable virtual host:
`ln -s /usr/local/etc/nginx/sites-available/mysite.local /usr/local/etc/nginx/sites-enabled/mysite.local`

Add `127.0.0.1 mysite.local` to `/etc/hosts`.

## Xhgui

Xhqui is a graphical interface for XHProf data built on MongoDB.

Update you mongodb configuration (username, password, host and/or port) if you're not using the default values:

    /usr/local/Cellar/php54-xhgui/ee00acb/web/config/config.php

Add indexes to mongodb for increased for performance:

    $ mongo xhprof
    db.results.ensureIndex( { 'meta.SERVER.REQUEST_TIME' : -1 } )
    db.results.ensureIndex( { 'profile.main().wt' : -1 } )
    db.results.ensureIndex( { 'profile.main().mu' : -1 } )
    db.results.ensureIndex( { 'profile.main().cpu' : -1 } )
    db.results.ensureIndex( { 'meta.url' : 1 } )

For system wide random profiling, you can add the following directive in php.ini:

    auto_prepend_file=/usr/local/Cellar/php54-xhgui/ee00acb/external/header.php

If you prefer to configure profiling per virtual host:

    server {
      # ..
      fastcgi_param PHP_VALUE "auto_prepend_file=/usr/local/Cellar/php54-xhgui/ee00acb/external/header.php";
    }

## Default Settings

### MySQL
Username: root

Password:

Port: 3306

### Xdebug
Remote port is 9002

## Configs

**Nginx**: `/usr/local/etc/nginx/nginx.conf`

**FastCGI**: `/usr/local/etc/nginx/fastcgi.conf`

**PHP**: `/usr/local/etc/php/5.4/php.ini`

**Redis**: `/usr/local/etc/redis.conf`

**Xdebug**: `/usr/local/etc/php/5.4/conf.d/ext-xdebug.ini`

**xhprof**: `/usr/local/etc/php/5.4/conf.d/ext-xhprof.ini`

Source code: `/usr/local/Cellar/php54-xhprof/`

**xhgui**: `/usr/local/Cellar/php54-xhgui/ee00acb/web/config/config.php`

**MySQL**: `$(brew --prefix mysql)/my.cnf`

### Handy projects
* [launchrocket](https://github.com/jimbojsb/launchrocket) - a PrefPane to manage all your Homebrew-installed services.
* [Hosts.prefpane](https://github.com/specialunderwear/Hosts.prefpane) - a PrefPane to manage /etc/hosts

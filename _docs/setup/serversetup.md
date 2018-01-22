---
title: "Server Setup"
permalink: "/docs/setup/serversetup/"
---

First setup the server - the clients use the server to pull down the installation scripts.


On the server
---

 1. Use git to checkout the latest version or download the zip file and put all files in the root directory of your website (for subdirs, see below).
 2. Create config.php in the root directory of your website. Make sure this file starts with `<?php` and has no whitespace before that, don't add the closing tag for php. config.php overrides the settings in config_default.php. To configure, simply copy relevant settings over from config_default.php to config.php and make the changes in config.php.
 3. Database setup: when using SQLite as backend (which is the default), check if the directory /app/db/ is writeable by the webserver. If you want to use MySQL, check config_default.php for the proper values.

 You're done with the server.

Create the first user
---

 1. Visit the site with a webbrowser, you'll be prompted to create a user and password
 2. Append the generated hash line to config.php
 3. Now refresh the page in your browser, and you should be able to log in with the credentials you just created.
 4. Additional local users may be added by going to http://server/munkireport-php/index.php?/auth/generate and copying the resultant configuration string -- $auth_config['newaccountname'] = '$P$B3n.QBh5pRaJHUBN6MZFNW4JyEjLbE.'; into the config.php file.  This is particularly useful when setting up business units.


#### No authentication

If you want to deploy munkireport without authentication (because you run your own authentication method), add the following line to config.php
`$conf['auth']['auth_noauth'] = array();`

Select which modules to install on the client
---

By default munkireport will only install 2 basic reporting modules: 'machine' and 'reportdata'. If you want the client to report on more items, visit:

 `http://example.com/index.php?/install/dump_modules/config`

Paste the resulting `$conf['modules'] = array(...);` in your config.php file. Remove items that you don't need reporting on from the array (e.g. 'servermetrics', 'certificate' and 'service' only make sense when installed on a client that runs Mac OSX server).

Descriptions of the [[Modules]].

## Advanced server setup
___

### Running MunkiReport from a subdir

Munkireport should able to detect if it is running from a subdirectory. If automatic detection fails, you can also specify the subdirectory in config.php.
So if you want to run munkireport from http://munki.mysite.org/report/
add the following to config.php:

    $conf['subdirectory'] = '/report/';


#### A simple Apache .vhost config to get you started

    <VirtualHost *:80>
      ServerAdmin webmaster@example.com
      ServerName  munkireport-php.example.com
      ServerAlias munkireport-php.example.com

      AddDefaultCharset utf-8
      DocumentRoot /srv/munkireport-php
        <Directory />
            Options FollowSymLinks
        </Directory>
      LogLevel warn
      CustomLog /var/log/apache2/munkireport-php.example.com-access.log combined
      ErrorLog /var/log/apache2/munkireport-php.example.com-error.log
    </VirtualHost>


#### Running MunkiReport with mod_rewrite

If you're running munkireport on an apache webserver and you want to use mod_rewrite (which gives you nicer urls), you'll have
to change the following:

 1. Add `$conf['index_page'] = '';` to config.php

#### NGINX on Ubuntu

Be sure to install these packages: `apt-get install php-fpm php-xml php-sqlite3 libcurl3-dev`

A Sample location block for NGINX
```
  location /munkireport-php/ {
    #If you are hosting outside of webroot
    alias /media/Data/munkireport-php/;
    autoindex on;
    location ~* \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        fastcgi_param SCRIPT_FILENAME  /media/Data/$fastcgi_script_name;
      }
  }
```

---
title: "Quick Demo V3"
permalink: "/docs/setup/quickdemov3/"
---
You can run a quick demo on your local machine to see what munkireport is all about. The only thing you need is a macOS machine with munki installed, a terminal (Terminal.app) and a web browser.
It uses the built-in web server of php, which is not very secure or robust but will suffice for the purpose of a quick demonstration and for development purposes.

## Get munkireport

Get the latest wip: <https://github.com/munkireport/munkireport-php/archive/wip.zip> Unpack the zip file (if the browser didn't do that for you).

## Get into the terminal

Open Terminal.app (it's in the /Applications/Utilities folder). In the terminal window, we need to step inside the downloaded munkireport folder. Type

```sh
cd Downloads/munkireport-php-wip/
```

## Install dependencies

MunkiReport V3 uses a dependency manager called `composer`. Make sure you have composer installed before you proceed. Instructions on how to install composer can be found on the [Composer Website](https://getcomposer.org/doc/00-intro.md). After you have installed composer on your system, run:

```sh
composer install --no-dev --no-suggest
```

This wil get all the dependencies for MunkiReport. To generate the autoload files, run

```sh
composer dump-autoload --optimize --no-dev
```

## Create the config file

MunkiReport needs a configuration file that contains all your local configuration items. For the demo we're creating a minimal configuration. Type:

```sh
echo "<?php \$conf['auth']['auth_noauth'] = [];" > config.php
```
to create a basic config file with no authentication

## Initialize the database

To initialise the database, you first need to create an empty database file:

```
touch app/db/db.sqlite
```

then you need to run the migration script:

```sh
php database/migrate.php
```

## Start the development server

Now we can start the web server. Type:

```sh
php -S localhost:8888 --docroot public
```
This will start the php development server on port 8888

## Visit the website

Now you can visit <http://localhost:8888>

## Configure the client

We're going to use your machine as client, type

```sh
sudo /bin/bash -c "$(curl -s http://localhost:8888/index.php?/install)"
```

This will install and configure munkireport on your local machine.
Now run

```sh
sudo managedsoftwareupdate
```

to populate your munkireport server with client data

## Stop the server

If you want to stop the server, type Ctrl-C to quit

## Uninstall munkireport

To uninstall munkireport just remove the installed files:

```sh
sudo rm /usr/local/munki/munkilib/reportcommon.*
sudo rm -r /usr/local/munki/preflight*
sudo rm -r /usr/local/munki/postflight*
sudo rm /usr/local/munki/report_broken_client
sudo rm -rf /usr/local/munki/munkireport*
sudo rm /Library/Preferences/MunkiReport.plist
```

---
title: "Quick Demo"
permalink: "/docs/quickdemo/"
---
You can run a quick demo on your local machine to see what munkireport is all about. The only thing you need is a macOS machine with munki installed, a terminal (Terminal.app) and a web browser.
It uses the built-in web server of php, which is not very secure or robust but will suffice for the purpose of a quick demonstration and for development purposes.

## Get munkireport

Visit https://github.com/munkireport/munkireport-php/releases/latest and scroll to the bottom of the page.
Click on the 'Source code (zip)' Download to get the latest version. Unpack the zip file (if the browser didn't do that for you).

## Create the config file

Open Terminal.app (it's in the /Applications/Utilities folder). In the terminal window, we need to step inside the downloaded munkireport folder. Type

```sh
cd Downloads/munkireport-php-2.6.0/
```
Adjust the version number to the correct one. Type

```sh
echo "<?php \$conf['auth']['auth_noauth'] = array();" > config.php
```
to create a basic config file with no authentication

## Start the development server

Now we can start the web server. Type

```sh
php -d short_open_tag=on -S localhost:8888
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

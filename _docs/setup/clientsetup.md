---
title: "Client Setup"
permalink: "/docs/setup/clientsetup/"
---
Munkireport is versatile in how you are able to add client machines to the reporting server. The manual option is used mostly for testing; the next method is the recommended way to distribute the setup to client machines.

The installation script is generated on the server and takes the following options:

```sh
Usage: munkireport_client_installer [OPTIONS]

  -b URL    Base url to munki report server
            Current value: https://YOUR.MUNKIREPORT.SERVER/
  -m PATH   Path to installation directory
            Current value: /usr/local/munki/
  -p PATH   Path to preferences file (without the .plist extension)
            Current value: /Library/Preferences/MunkiReport
  -n        Do not run preflight script after the installation
  -i PATH   Create a full installer at PATH
  -c ID     Change pkg id to ID
  -h        Display this help message
  -r PATH   Path to installer result plist
  -v VERS   Override version number
```

## Examples

### Commandline (Manual) install

The simplest form to install the client scripts is to run the following line on the client:

1. Open Terminal.app
2. Type: `sudo /bin/bash -c "$(curl -s https://example.com/index.php?/install)"`

This will download the installscript with curl and run it with root privileges. The install script will download all additional reporting scripts and configure the client to report back to your munkireport server.

**Note:** Use only for testing purposes.

### Generate an installer package (pkg)

When the manual test above goes well, you can create an installer package and add it to your munki repo:

1. Open Terminal.app
2. Create the installer package: `bash -c "$(curl http://example.com/index.php?/install)" bash -i ~/Desktop`

You will end up with a munkireport installer package on your desktop (~/Desktop/munkireport-x.x.x.pkg)
You can then distribute this package with your favourite distribution mechanism. But why not import this package into munki?

1. Run `/usr/local/munki/munkiimport ~/Desktop/munkireport-x.x.x.pkg` (changing the version number as needed).
2. Run `makecatalogs`, and be sure to add the newly imported package to a manifest.

### Version override

If you need to push out a new version to your clients (maybe you changed some client side options in the config.php), but the version of munkireport has not changed, you can override the version using the `-v` flag:

```sh
bash -c "$(curl http://example.com/index.php?/install)" bash -v 2.9.2.20161005 -i ~/Desktop
```

This will result in ~/Desktop/munkireport-2.9.2.20161005.pkg

You can import this package into munki which will pick up the new version number.

### AutoPkg

There is also an AutoPkg recipe for creating munkireport packages, you can read more on how to setup those on [AutoPkg for MunkiReport](../upgrading/autopkgformunkireport).


### Advanced client setup

When MunkiReport is installed on the client, 3 directories are generated:

1. `preflight.d` - this directory is used by munkireport to run scripts on preflight, it contains at least `submit.preflight`. Scripts that exit with a non-zero status will not abort the run.
3. `preflight_abort.d` - this directory is empty and can be used for additional scripts that check if managedsoftwareupdate should run. Scripts that exit with a non-zero status will abort the run.
4. `postflight.d` - this directory is empty and can be used for additional scripts that should run on postflight.

All scripts have a timeout limit of 10 seconds, after that they're killed.

---
title: "Server Configuration"
permalink: "/docs/configuration/serverconfiguration/"
---
Slightly modified from the original source: `config_default.php`

Don't edit `config_default.php` file directly, it will get overwritten when a new version gets out. Just add the items you want to change to `config.php` and change them there.

### Index page

Default is `index.php?` which is the most compatible form. You can leave it blank if you want nicer looking urls. You will need a server which honors `.htaccess` (Apache) or figure out how to rewrite urls in the server of your choice.

Default:

```
$conf['index_page'] = 'index.php?';
```

### Uri protocol

`$_SERVER` variable that contains the correct request path, e.g. `REQUEST_URI`, `QUERY_STRING`, `PATH_INFO`, etc. Defaults to `AUTO`.

Default:

```
$conf['uri_protocol'] = 'AUTO';
```

### HTTP host

The hostname of the webserver, default automatically determined. No trailing slash.

Default:

```
$conf['webhost'] = (empty($_SERVER['HTTPS']) ? 'http' : 'https')
		. '://'.$_SERVER[ 'HTTP_HOST' ];
```

### Subdirectory

Relative to the webroot, with trailing slash. If you're running munkireport from a subdirectory of a website, enter subdir path here. E.g. if munkireport is accessible here: `http://mysite/munkireport/` you should set subdirectory to `'/munkireport/'`

If you're using `.htaccess` to rewrite urls, you should change that too. The code below is for automagically deterimining your subdirectory, if it fails, just add `$conf['subdirectory'] = '/your_sub_dir/'` in `config.php`

Default:

```
$conf['subdirectory'] = substr(
					    $_SERVER['PHP_SELF'],
					    0,
					    strpos($_SERVER['PHP_SELF'], basename(FC))
				    );
```

### Sitename

Will appear in the title bar of your browser and as heading on each webpage.

Default:

```
$conf['sitename'] = 'MunkiReport';
```

### Authentication

Currently four authentication methods are supported.

#### 1. Don't require any authentication

Paste the following line in your `config.php`:

```
$conf['auth']['auth_noauth'] = array();
```

#### 2. (default) Local accounts

Visit `/index.php?/auth/generate` and paste the result in your `config.php`.

#### 3. LDAP

At least fill in these items:

```
$conf['auth']['auth_ldap']['server']      = 'ldap.server.local'; // One or more servers separated by commas.
$conf['auth']['auth_ldap']['usertree']    = 'uid=%{user},cn=users,dc=server,dc=local'; // Where to find the user accounts.
$conf['auth']['auth_ldap']['grouptree']   = 'cn=groups,dc=server,dc=local'; // Where to find the groups.
$conf['auth']['auth_ldap']['mr_allowed_users'] = array('user1','user2'); // For user based access, fill in users.
$conf['auth']['auth_ldap']['mr_allowed_groups'] = array('group1','group2'); // For group based access, fill in groups.
```

Optional items:

```
$default_conf['userfilter']  = '(&(uid=%{user})(objectClass=posixAccount))'; // LDAP filter to search for user accounts.
$default_conf['groupfilter'] = '(&(objectClass=posixGroup)(memberUID=%{uid}))'; // LDAP filter to search for groups.
$conf['auth']['auth_ldap']['port']        = 389; // LDAP port.
$conf['auth']['auth_ldap']['version']     = 3; // Use LDAP version 1, 2 or 3.
$conf['auth']['auth_ldap']['starttls']    = FALSE; // Set to TRUE to use TLS.
$conf['auth']['auth_ldap']['referrals']   = FALSE; // Set to TRUE to follow referrals.
$conf['auth']['auth_ldap']['deref']       = LDAP_DEREF_NEVER; // How to dereference aliases. See http://php.net/ldap_search
$conf['auth']['auth_ldap']['binddn']      = ''; // Optional bind DN
$conf['auth']['auth_ldap']['bindpw']      = ''; // Optional bind password
$conf['auth']['auth_ldap']['userscope']   = 'sub'; // Limit search scope to sub, one or base.
$conf['auth']['auth_ldap']['groupscope']  = 'sub'; // Limit search scope to sub, one or base.
$conf['auth']['auth_ldap']['groupkey']    = 'cn'; // The key that is used to determine group membership
$conf['auth']['auth_ldap']['debug']       = 0; // Set to TRUE to debug LDAP.
```

Example of LDAP Authentication Against Active Directory:

```
$conf['auth']['auth_ldap']['server']      = 'ldap.ad.myorg.com'; // One or more servers separated by commas.
$conf['auth']['auth_ldap']['usertree']    = 'ou=PersonOU,dc=ad,dc=myorg,dc=com'; // Where to find the user accounts.
$conf['auth']['auth_ldap']['grouptree']   = 'ou=GroupOU,dc=ad,dc=myorg,dc=com'; // Where to find the groups.
$conf['auth']['auth_ldap']['mr_allowed_users'] = array('user1','user2'); // For user based access, fill in users.
$conf['auth']['auth_ldap']['mr_allowed_groups'] = array('group1','group2'); // For group based access, fill in groups.
$conf['auth']['auth_ldap']['userfilter']  = '(&(cn=%{user})(objectClass=user))'; // LDAP filter to search for user accounts.
$conf['auth']['auth_ldap']['groupfilter'] = '(&(objectClass=group)(member=%{dn}))';
$conf['auth']['auth_ldap']['starttls']    = TRUE; // Set to TRUE to use TLS.
$conf['auth']['auth_ldap']['binddn']      = 'MYDOMAIN\mysecretbindusername'; // Optional bind DN
$conf['auth']['auth_ldap']['bindpw']      = 'mysecretbindpwd'; // Optional bind password
```

#### 4. Active Directory

Fill the needed and include the lines in your `config.php`, e.g:

```
$conf['auth']['auth_AD']['account_suffix'] = '@mydomain.local';
$conf['auth']['auth_AD']['base_dn'] = 'DC=mydomain,DC=local'; //set to NULL to auto-detect
$conf['auth']['auth_AD']['domain_controllers'] = array('dc01.mydomain.local'); //can be an array of servers
$conf['auth']['auth_AD']['admin_username'] = NULL; //if needed to perform the search
$conf['auth']['auth_AD']['admin_password'] = NULL; //if needed to perform the search
$conf['auth']['auth_AD']['mr_allowed_users'] = array('macadmin','bossman');
$conf['auth']['auth_AD']['mr_allowed_groups'] = array('AD Group 1','AD Group 2'); //case sensitive
```

Authentication methods are checked in the order that they appear above. Not in the order of your `config.php`! You can combine methods 2, 3 and 4.

### Role Based Authorization

Authorize actions by listing roles appropriate array. Don't change these unless you know what you're doing, these roles are also used by the Business Units.

Default:

```
$conf['authorization']['delete_machine'] = array('admin', 'manager');
$conf['authorization']['global'] = array('admin');
```

### Roles

Add users or groups to the appropriate roles array.

Default:

```
$conf['roles']['admin'] = array('*');
```

### Local groups

Create local groups, add users to groups.

Example:

```
$conf['groups']['admin_users'] = array();
```

### Business Units

Set to `TRUE` to enable Business Units. For more information, see `docs/business_units.md`.

Default:

```
$conf['enable_business_units'] = FALSE;
```

### Force secure connection when authenticating

Set this value to `TRUE` to force https when logging in. This is useful for sites that serve MR both via http and https.

Default:

```
$conf['auth_secure'] = FALSE;
```

### VNC and SSH links, optional links in the client detail view

If you want to have link that opens a screensharing or SSH connection to a client, enable these settings. If you don't want the links, set either to an empty string, e.g:

`$conf['vnc_link'] = "";`

Default:

```
$conf['vnc_link'] = "vnc://%s:5900";
$conf['ssh_link'] = "ssh://adminuser@%s";
```

### Inventory - bundle ignore list

List of bundle-id's to be ignored when processing inventory. The list is processed using regex, examples:

Skip  all virtual windows apps created by parallels and VMware:

```
$conf['bundleid_ignorelist'][] = array('com.parallels.winapp.*', 'com.vmware.proxyApp.*');
```

Skip all Apple apps, except iLife, iWork and Server:

```
'com.apple.(?!iPhoto)(?!iWork)(?!Aperture)(?!iDVD)(?!garageband)(?!iMovieApp)(?!Server).*'
```

Skip all apps with empty bundle-id's:

```
'^$'
```

Default:

```
$conf['bundleid_ignorelist'][] = 'com.parallels.winapp.*';
$conf['bundleid_ignorelist'][] = 'com.vmware.proxyApp.*';
```

### Inventory - path ignore list

List of bundle-paths to be ignored when processing inventory.
The list is processed using regex, examples:

Skip all apps in `/System/Library`:

```
$conf['bundlepath_ignorelist'][] = '/System/Library/.*';
```

Skip all apps that are contained in an app bundle:

```
$conf['bundlepath_ignorelist'][] = '.*\.app\/.*\.app'
```

Default:

```
$conf['bundlepath_ignorelist'] = array('/System/Library/.*');
```

### Modules

List of modules that have to be installed on the client.

See for possible values the names of the directories in `app/modules/`
e.g. `$conf['modules'] = array('disk_report', 'inventory');`

An empty list installs only the basic reporting modules, Machine and Reportdata.

Default:

```
$conf['modules'] = array('munkireport');
```

### Displays module history option

By default this module overrides the information of a client computer on each client's report submission.

If you would like to keep displays information until the display is seen again on a different computer use:

```
$conf['keep_previous_displays'] = TRUE;
```

When not configured, or if set to `FALSE`, the default behaviour applies.

Example:

```
$conf['keep_previous_displays'] = TRUE;
```

### Unit of temperature °C or °F

Unit of temperature, possible values: F for Fahrenheit, C for Celsius

```
$conf['temperature_unit'] = 'F';
```

When not configured, the default behaviour applies. By default temperature units are displayed in Celsius °C.

Example:

```
$conf['temperature_unit'] = 'F';
```

### Migrations

When a new version of munkireport comes out it might need to update your database structure if you want to allow this, set

```
$conf['allow_migrations'] = TRUE;
```

There is a small overhead (one database query) when setting allow_migrations to TRUE. If you are concerned about performance, you can set allow_migrations to FALSE when you're done migrating.

Default:

```
$conf['allow_migrations'] = FALSE;
```

### Client passphrases

List of passphrases that the client can use to authenticate

On the client:

`defaults write /Library/Preferences/MunkiReport Passphrase 'secret1'`

On the server:

```
$conf['client_passphrases'] = array('secret1', 'secret2');
```

Default:

```
$conf['client_passphrases'] = array();
```

### Proxy settings

If you are behind a proxy, MunkiReport may be unable to retrieve warranty and model information from Apple.

Note that there is only authenticated proxy support for basic authentication.

```
$conf['proxy']['server'] = 'proxy.yoursite.org'; // Required
$conf['proxy']['username'] = 'proxyuser'; // Optional
$conf['proxy']['password'] = 'proxypassword'; Optional
$conf['proxy']['port'] = 8080; // Optional, defaults to 8080
```

Example:

```
$conf['proxy']['server'] = 'proxy.yoursite.org';
```

### Request timeout

Timeout for retrieving warranty and model information from Apple.

Timeout in seconds.

Default:

```
$conf['request_timeout'] = 5;
```

### Dashboard - IP Ranges

Plot IP ranges by providing an array with labels and a partial IP address. Specify multiple partials in array if you want to group them together. The IP adress part is queried with SQL LIKE

Examples:

```
$conf['ip_ranges']['MyOrg'] = '100.99.';
$conf['ip_ranges']['AltLocation'] = array('211.88.12.', '211.88.13.');
```

Default:

```
$conf['ip_ranges'] = array();
```

### Dashboard - VLANS

Plot VLANS by providing an array with labels and a partial IP address of the routers. Specify multiple partials in array if you want to group them together. The router IP address part is queried with SQL LIKE

Examples:

```
$conf['ipv4routers']['Wired'] = '211.88.10.1';
$conf['ipv4routers']['WiFi'] = array('211.88.12.1', '211.88.13.1');
$conf['ipv4routers']['Private range'] = array('10.%', '192.168.%',
 	'172.16.%',
 	'172.17.%',
 	'172.18.%',
 	'172.19.%',
 	'172.2_.%',
 	'172.30.%',
 	'172.31.%', );
$conf['ipv4routers']['Link-local'] = array('169.254.%');
```

### Dashboard - Layout

Dashboard layout is an array of rows that contain an array of widgets. Omit the `_widget` postfix

Up to three small horizontal widgets will show on one line

Up to two medium horizontal widgets will show on one line

Responsive horizontal widgets will change depending on window size

Be aware of medium / dynamic vertical widgets as it may skew the responsive design

This is a list of the current dashboard widgets

Small horizontal widgets:

* bound_to_ds
* client (two items)
* external_displays_count
* hardware_model
* smart_status
* disk_report
* uptime
* installed_memory
* munki
* power_battery_condition
* power_battery_health

Small horizontal / medium vertical widgets:

* network_location

Small horizontal / dynamic vertical widgets:

* app
* duplicated_computernames
* filevault
* hardware_model
* manifests
* modified_computernames
* munkiinfo_munkiprotocol
* munki_versions
* new_clients
* pending
* pending_munki
* pending_apple
* warranty
* wifi_networks
* wifi_state

Medium horizontal widgets:

Medium horizontal / dynamic vertical widgets:

* hardware_age
* hardware_model
* memory
* os
* printer
* messages

Responsive horizontal widgets:

* network_vlan
* registered_clients

Default:

```
$conf['dashboard_layout'] = array(
	array('client', 'munki'), /*client is actually two widgets*/
	array('disk_report', 'installed_memory', 'bound_to_ds'),
	array('uptime', 'pending_apple', 'pending_munki'),
	array('new_clients', 'munki_versions', 'filevault'),
	array('warranty','power_battery_condition','power_battery_health'),
	array('bluetooth_battery')
);
```

### Apps Version Report

List of applications, by name, that you want to see in the apps version report. If this is not set the report page will appear empty. This is case insensitive but must be an array.

E.g.:

```
$conf['apps_to_track'] = array('Flash Player', 'Java', 'Firefox', 'Microsoft Excel');
```

Default:

```
$conf['apps_to_track'] = array('Safari');

```

### Disk Report Widget Thresholds

Thresholds for disk report widget. This array holds two values:

* free gigabytes below which the level is set to 'danger'
* free gigabytes below which the level is set as 'warning'

If there are more free bytes, the level is set to 'success'

Default:

```
$conf['disk_thresholds'] = array('danger' => 5, 'warning' => 10);
```

### App settings

If the webapp is in a different directory as index.php, adjust the variables below. For enhanced security it is advised to put the webapp in a directory that is not visible to the internet.

Default:

```
// Path to system folder, with trailing slash
$conf['system_path'] = APP_ROOT.'/system/';

// Path to app folder, with trailing slash
$conf['application_path'] = APP_ROOT.'/app/';

// Path to view directory, with trailing slash
$conf['view_path'] = $conf['application_path'].'views/';

// Path to controller directory, with trailing slash
$conf['controller_path'] = $conf['application_path'].'controllers/';

// Path to modules directory, with trailing slash
$conf['module_path'] = $conf['application_path'] . "modules/";

// Routes
$conf['routes'] = array();
$conf['routes']['module(/.*)?']	= "module/load$1";
```

### PDO Datasource

Specify dsn, username, password and options.

Supported engines: sqlite and mysql.

MySQL example:

```
$conf['pdo_dsn'] = 'mysql:host=localhost;dbname=munkireport';
$conf['pdo_user'] = 'munki';
$conf['pdo_pass'] = 'munki';
$conf['pdo_opts'] = array(PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES utf8');
```

Default:

```
$conf['pdo_dsn'] = 'sqlite:'.$conf['application_path'].'db/db.sqlite';
$conf['pdo_user'] = '';
$conf['pdo_pass'] = '';
$conf['pdo_opts'] = array();
```

### Create table options

For MySQL, define the default table and charset.

Default:

```
$conf['mysql_create_tbl_opts'] = 'ENGINE=InnoDB DEFAULT CHARSET=utf8';
```

### Timezone

See http://www.php.net/manual/en/timezones.php for valid values.

Default:

```
$conf['timezone'] = @date_default_timezone_get();
```

### Custom css and js

If you want to override the default css or default js, you can specify a custom file that will be included in the header (css) and footer (js):

Example:

```
$conf['custom_css'] = '/custom.css';
$conf['custom_js'] = '/custom.js';
```

### Debugging

If set to `TRUE`, will deliver debugging messages in the page. Set to `FALSE` in a production environment.

Default:

```
$conf['debug'] = FALSE;
```

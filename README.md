check_syncthing
===============

# What it does

This plugin for Nagios/Icinga/Icinga 2 monitors the connected devices by using the provied REST API.
This can be usefull if you have a "master" Syncthing client running somewhere which requires some clients to be connected.

You can NOT monitor if a specific client is connected. Only the overall amount of connected clients can be monitored.

This plugin is developed according to the "Nagios Developer Guidelines" which can be found here: https://nagios-plugins.org/doc/guidelines.html
Therefore it can be used by any monitoring systems which claims to be Nagios compatible.

# CheckCommand for Icinga 2

A sample CheckCommand for Icinga 2 can be found in ```icinga2_checkCommand.conf```.

# Plugin arguments

The script takes the following arguments

CheckCommand Variable | Plugin Argument | Required | Description
----------------------|-----------------|----------|------------
syncthing_url | --syncthing-url | yes | The URL to syncthing. For example: https://sync.veloc1ty.lan
syncthing_http_user | --http-user | no | The HTTP Basic auth user
syncthing_http_pass | --http-pass | no | The HTTP Basic auth password
syncthing_min_connected_endpoints_warn | no | --min-connected-endpoints-warn | Minimum connected endpoints. If below trigger WARNING. Default: 1
syncthing_min_connected_endpoints_crit | no | --min-connected-endpoints-crit | Minimum connected endpoints. If below trigger CRITICAL. Default: 0

Note: The URL is without the trailing '/'

# HTTP Basic Authentification #

Some Syncthing applications needs HTTP Basic Auth before a query can be sent. Set the ```syncthing_http_user``` or ```syncthing_http_pass``` argument to enable authentification.

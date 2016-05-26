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
syncthing_api_key | --api-key | yes | Your API key from the settings menu
syncthing_min_connected_endpoints_warn | --min-connected-endpoints-warn | no | Minimum connected endpoints. If below trigger WARNING. Default: 1
syncthing_min_connected_endpoints_crit | --min-connected-endpoints-crit | no | Minimum connected endpoints. If below trigger CRITICAL. Default: 0

Note: The URL is without the trailing '/'

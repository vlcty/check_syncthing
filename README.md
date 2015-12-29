check_syncthing
===============

# What it does

This script monitors the connections of your syncthing application by using
the built in REST API.

# How to install

Like every other Nagios/Icinga plugin. A sample CheckCommand for Icinga 2 can be
found in the file ```icinga2_checkCommand.conf```.

# Arguments

The script takes a few arguments:

CheckCommand Variable | Plugin Argument | Description
----------------------|-----------------|-------------
syncthing_url | --syncthing-url | The URL to syncthing. For example: https://sync.veloc1ty.lan
syncthing_http_user | --http-user | The HTTP Basic auth user (optional)
syncthing_http_pass | --http-pass | The HTTP Basic auth password (optional)
syncthing_min_connected_endpoints_warn | --min-connected-endpoints-warn | Minimum connected endpoints. If below trigger WARNING
syncthing_min_connected_endpoints_crit | --min-connected-endpoints-crit | Minimum connected endpoints. If below trigger CRITICAL

Note: The URL is without the trailing '/'

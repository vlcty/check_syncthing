check_syncthing
===============

# What it does

This plugin for Nagios/Icinga/Icinga 2 monitors the connected devices by using the provied REST API.
This can be usefull if you have a "master" Syncthing client running somewhere which requires some clients to be connected.

You can use it in two ways:
* Overall mode: Monitor overall connected devices and warn if they go under a specific limit
* Specific device: Monitor if a specific device is configured and connected

It produces output like:

```
OK - 3 of 8 endpoints are connected
```

or

```
OK - Device AAAAAA-IADQBQT-7J5YROQ-3JMJAV7-S5QUAMF-LF4DV3P-N4MU3KL-XXXXXX is connected and in sync
```

This plugin is developed according to the "Nagios Developer Guidelines" which can be found here: https://nagios-plugins.org/doc/guidelines.html

Therefore it can be used by any monitoring systems which claims to be Nagios compatible.

# CheckCommand for Icinga 2

A suitable CheckCommand for Icinga 2 can be found in ```icinga2_checkCommand.conf```.

The integration is really simple. I've added a hash in my syncthing "master" host definition containing the node name as key and the device ID in the body.

```
vars.syncthing["dns1"] = {
    syncthing_device_id = "thedeviceidofdns1"
}

vars.syncthing["homeserver"] = {
    syncthing_device_id = "thedeviceidofmyhomeserver"
}
```

It's then very easy to build a apply rule to match that entries.

```
apply Service "syncthing-" for ( node => config in host.vars.syncthing ) {
    import "15minute-service"
    import "notification-enabled"
    import "no-perfdata"

    display_name = "Syncthing: " + node
    check_command = "syncthing"
    command_endpoint = NodeName

    vars.syncthing_url = "https://mysyncthingserver"
    vars.syncthing_api_key = "myapikey"
    vars += config
}
```

# Plugin arguments

To integrate it in other systems here are the command line argument the script takes:

CheckCommand Variable | Plugin Argument | Required | Description
----------------------|-----------------|----------|------------
syncthing_url | --syncthing-url | yes | The URL to syncthing. For example: https://sync.veloc1ty.lan
syncthing_api_key | --api-key | yes | Your API key from the settings menu
syncthing_device_id | --device-id | no | The device ID to be looked for
syncthing_min_connected_endpoints_warn | --min-connected-endpoints-warn | no | Minimum connected endpoints. If below trigger WARNING. Default: 1
syncthing_min_connected_endpoints_crit | --min-connected-endpoints-crit | no | Minimum connected endpoints. If below trigger CRITICAL. Default: 0

Note: The URL is without the trailing '/'

# Examples

## Example 1: Check the overall connected endpoints

Let's say you want to monitor how many devices are conencted. A call could look like this:

```
check_syncthing --syncthing-url="https://sync.veloc1ty.lan" --api-key="1234"
```

It would be WARNING when only one device is connected and be CRITICAL if no device is connected anymore. This is the default mode.

You can adjust the limits with ```----min-connected-endpoints-warn``` and ```--min-connected-endpoints-crit```.

## Example 2: Check if a specific device is connected

This mode checks the connection state of a single device. This mode can be forced by using ```--device-id```. A call could look like:

```
check_syncthing --syncthing-url="https://sync.veloc1ty.lan" --api-key="1234" --device-id="LV6W6OG-45CE7QN-V25URFY-HWSEJHC-5TZWE7L-Z3FQY6G-7ATKMBS-ECYPH4L"
```

If the device is not known or not connected it'll go into CRITICAL state.

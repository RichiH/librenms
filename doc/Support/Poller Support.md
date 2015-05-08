### poller.php

This document will explain how to use poller.php to debug issues or manually running to process data.

#### Command options
```bash
	LibreNMS 2014.master Poller

-h <device id> | <device hostname wildcard>  Poll single device
-h odd                                       Poll odd numbered devices  (same as -i 2 -n 0)
-h even                                      Poll even numbered devices (same as -i 2 -n 1)
-h all                                       Poll all devices

-i <instances> -n <number>                   Poll as instance <number> of <instances>
                                             Instances start at 0. 0-3 for -n 4

Debugging and testing options:
-r                                           Do not create or update RRDs
-d                                           Enable debugging output
-m                                           Specify module(s) to be run
```

`-h` Use this to specify a device via either id or hostname (including wildcard using *). You can also specify odd and 
even. all will run poller against all devices.

`-i` This can be used to stagger the poller process.

`-r` This option will suppress the creation or update of RRD files.

`-d` Enables debugging output (verbose output) so that you can see what is happening during a poller run. This includes 
things like rrd updates, SQL queries and response from snmp.

`-m` This enables you to specify the module you want to run for poller.

#### Poller config

These are the default poller config items. You can globally disable a module by setting it to 0. If you just want to 
disable it for one device then you can do this within the WebUI -> Settings -> Modules.

```php
$config['poller_modules']['unix-agent']                   = 0;
$config['poller_modules']['system']                       = 1;
$config['poller_modules']['os']                           = 1;
$config['poller_modules']['ipmi']                         = 1;
$config['poller_modules']['sensors']                      = 1;
$config['poller_modules']['processors']                   = 1;
$config['poller_modules']['mempools']                     = 1;
$config['poller_modules']['storage']                      = 1;
$config['poller_modules']['netstats']                     = 1;
$config['poller_modules']['hr-mib']                       = 1;
$config['poller_modules']['ucd-mib']                      = 1;
$config['poller_modules']['ipSystemStats']                = 1;
$config['poller_modules']['ports']                        = 1;
$config['poller_modules']['bgp-peers']                    = 1;
$config['poller_modules']['junose-atm-vp']                = 1;
$config['poller_modules']['toner']                        = 1;
$config['poller_modules']['ucd-diskio']                   = 1;
$config['poller_modules']['wifi']                         = 1;
$config['poller_modules']['ospf']                         = 1;
$config['poller_modules']['cisco-ipsec-flow-monitor']     = 1;
$config['poller_modules']['cisco-remote-access-monitor']  = 1;
$config['poller_modules']['cisco-cef']                    = 1;
$config['poller_modules']['cisco-sla']                    = 1;
$config['poller_modules']['cisco-mac-accounting']         = 1;
$config['poller_modules']['cipsec-tunnels']               = 1;
$config['poller_modules']['cisco-ace-loadbalancer']       = 1;
$config['poller_modules']['cisco-ace-serverfarms']        = 1;
$config['poller_modules']['netscaler-vsvr']               = 1;
$config['poller_modules']['aruba-controller']             = 1;
$config['poller_modules']['entity-physical']              = 1;
$config['poller_modules']['applications']                 = 1;
$config['poller_modules']['cisco-asa-firewall']           = 1;
```

#### Poller modules

`unix-agent`: Enable the check_mk agent for external support for applications.

`system`: Provides information on some common items like uptime, sysDescr and sysContact.

`os`: Os detection. This module will pick up the OS of the device.

`ipmi`: Enables support for IPMI if login details have been provided for IPMI.

`sensors`: Sensor detection such as Temperature, Humidity, Voltages + More.

`processors`: Processor support for devices.

`mempools`: Memory detection support for devices.

`storage`: Storage detection for hard disks

`netstats`: Statistics for IP, TCP, UDP, ICMP and SNMP.

`hr-mib`: Host resource support.

`ucd-mib`: Support for CPU, Memory and Load.

`ipSystemStats`: IP statistics for device.

`ports`: This module will detect all ports on a device excluding ones configured to be ignored by config options.

`bgp-peers`: BGP detection and support.

`junose-atm-vp`: Juniper ATM support.

`toner`: Toner levels support.

`ucd-diskio`: Disk I/O support.

`wifi`: WiFi Support for those devices with support.

`ospf`: OSPF Support.

`cisco-ipsec-flow-monitor': IPSec statistics support.

`cisco-remote-access-monitor`: Cisco remote access support.

`cisco-cef`: CEF detection and support.

`cisco-sla`: SLA detection and support.

`cisco-mac-accounting`: MAC Address account support.

`cipsec-tunnels`: IPSec tunnel support.

`cisco-ace-loadbalancer`: Cisco ACE Support.

`cisco-ace-serverfarms`: Cisco ACE Support.

`netscaler-vsvr`: Netscaler support.

`aruba-controller`: Arube wireless controller support.
 
`entity-physical`: Module to pick up the devices hardware support.

`applications`: Device application support.

`cisco-asa-firewall`: Cisco ASA firewall support.

#### Running

Here are some examples of running poller from within your install directory.
```bash
./poller.php -h localhost

./poller.php -h localhost -m ports
```

#### Debugging

To provide debugging output you will need to run the poller process with the `-d` flag. You can do this either against 
all modules, single or multiple modules:

All Modules
```bash
./poller.php -h localhost -d
```

Single Module
```bash
./poller.php -h localhost -m ports -d
```

Multiple Modules
```bash
./poller.php -h localhost -m ports,entity-physical -d
```

It is then advisable to sanitise the output before pasting it somewhere as the debug output will contain snmp details 
amongst other items including port descriptions.

The output will contain:

DB Updates

RRD Updates

SNMP Response
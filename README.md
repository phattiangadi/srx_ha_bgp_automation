# SRX HA failover based on BGP route status - SLAX automation

Slax script logic to take decisions based on BGP neighborship and RE status on an SRX in HA configuration.

## Installation Method.

1. Copy the script to the device under /var/tmp

2. Add the following configuration.

## Section 1 configuration - Generate a event every 60s.

#### set event-options generate-event every-60s time-interval 60 

## Monitor the event every 60s and execute the script.

#### set event-options policy bgp-check-script events every-60s 
#### set event-options policy bgp-check-script then event-script check_route_script.slax 
#### set event-options event-script file /var/tmp/check_route_script.slax

3. Based on the automation configuration and schedule, the script would run to monitor the BGP neighborship between devices and take appropriate action to bring down monitored interfaces on a HA environment so that failover can be triggered. Here are the conditions in which the failover is triggered.

    - If the upstream BGP neighborship is down and the default route received from upstream is not present.
    - Script will check for the default route and when the default route is not present in the routing table (RT), the monitored interface is administratively shutdown.
    - Shutdown of the administrative interface will trigger a device failover.
    - The script also considers condition whether the device RE is in master state before the failover is triggered.

    In all other cases not described above, no decision is taken to failover.

NOTE: The same scenario is applied to BGP adjacencies north bound or south bound.

In the check_route_script.slax, ensure to change the following parameters to match the neighborship and interfaces in the environment this is being implemented.

# Troubleshooting.

Script execution output can be viewed in /var/log/escript.log (Event based script log, this would provide information on the execution and any errors that is reported by the script)

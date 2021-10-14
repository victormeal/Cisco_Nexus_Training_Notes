# SPAN
----
````
Step 1. Configure the destination port, the port where you are going to capture the traffic.
switch# configure terminal
switch(config)# interface ethernet <x/x>
switch(config-if)# switchport
switch(config-if)# switchport monitor
switch(config-if)# no shut
switch(config-if)# exit
switch(config)#
````
````
Step2 (optional). Filter the traffic that is going to be mirrored on the destination port.
switch# configure terminal
switch(config)# ip access-list my_SPAN_ACL
switch(config-acl)# permit ip 10.64.101.81/32 10.22.19.81/32
switch(config-acl)# permit ip 10.22.19.81/32 10.64.101.81/32
switch(config-acl)# permit ip 10.64.100.81/32 10.22.18.81/32
switch(config-acl)# permit ip 10.22.18.81/32 10.64.100.81/32
switch(config-acl)# exit
switch(config)# vlan access-map my_SPAN_filter
switch(config-access-map)# match ip address my_SPAN_ACL
switch(config-access-map)# action forward
switch(config-access-map)# exit
````
````

Step 3. Configure the monitor session.
switch(config)# monitor session 1
switch(config-monitor)# source interface ethernet ethernet <y/y> rx
switch(config-monitor)# destination interface ethernet <x/x>
switch(config-monitor)# filter access_group my_SPAN_filter
switch(config-monitor)# no shut
switch(config-monitor)# exit
````
````
Step 4. Verify
switch# show monitor session 1
switch# show run monitor
switch# show ip access-lists my_SPAN_ACL
switch# show vlan access-map my_SPAN_filter
````
----


## SPAN Sources
	- Ethernet ports
	- A port configured as a source port cannot also be configured as a destination port

## Span Destinations
	- Ethernet ports in either access or trunk mode
	- A port configured as a destination port cannot also be configured as a source port.
	- A destination port can be configured in only one SPAN session at a time.
	- Destination ports do not participate in any spanning tree instance. SPAN output includes bridge protocol data unit (BPDU) Spanning Tree Protocol hello packets.

## Span Sessions

## Guidelines and Limitations for SPAN
	- All SPAN replication is performed in the hardware. The supervisor CPU is not involved.
	- You can configure only one destination port in a SPAN session.
	- Configuring two SPAN or ERSPAN sessions on the same source interface with only one filter is not supported. If the same source is used in multiple SPAN or ERSPAN sessions either all the sessions must have different filters or no sessions should have filters.
	- SPAN is not supported for management ports.
	- the same source can be part of multiple sessions.

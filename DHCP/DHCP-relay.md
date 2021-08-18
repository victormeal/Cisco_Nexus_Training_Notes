# DHCP Relay

```
feature dhcp

ip dhcp relay
ip dhcp relay source-interface <inteface>
interface ethernet x/y
 ip dhcp relay address x.x.x.x
```
```
Show ip dhcp relay
Show ip dhcp relay address
```
----
```
(optional) ip dhcp snooping vlan vlan-list

interface ethernet 2/1
 ip dhcp snooping trust

ip dhcp relay information option trust
interface ethernet 2/1
 ip dhcp relay information trusted


ip dhcp relay
(optional) ip dhcp relay information option vpn


interface ethernet 2/3
 ip dhcp relay address 10.132.7.120 use-vrf red

ip dhcp relay source-interface loopback 2
```
----
```
DHCP Relay configuration from UDE-CORE1 (vPC port-channel 1):

feature dhcp
ip dhcp snooping
ip dhcp snooping vlan 102,104,106,112,114,120
IP DHCP SNOOPING VERIFY MAC-ADDRESS
ip dhcp relay
int po1
ip dhcp snooping trust
exit
ip dhcp relay source-interface vlan 102

DHCP Relay configuration from UDE-CORE2:

feature dhcp
ip dhcp snooping
ip dhcp snooping vlan 102,104,106,112,114,120
IP DHCP SNOOPING VERIFY MAC-ADDRESS
ip dhcp relay
int e1/3 --> Interface facing DHCP Server
ip dhcp snooping trust
exit
ip dhcp relay source-interface vlan 102
```

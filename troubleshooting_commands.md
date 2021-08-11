# Cisco Nexus - Troubleshooting Commands
## Contents
- Basic Commands
- Hardware
- Hardware Forwarding
- L1
- FEX
- Performance Issues
- L2 Protocols
- L3 Unicast Protocols
- Multicast
- VXLAN
- MPLS
- QoS
- Management
- Monitoring
- Security
- System Management
- Licensing
- FC/FCoE
- Automation/Programmability
- DME Inconsistency
- Miscellaneous
- Packet Capture

----
## Basic Commands
- `show run`
- `show run all`
- `show logging log`
- `show logging level`
- `show logging level <something>`
----
## Hardware
- `show enviroment`
- `show inventory`
- `show module`
- `show hardware profile status`
- `show interface e1/1 capabilities`
- `show environment temperature `
### Sup engines
### Linecards
### Chassis (TOR)
### Chassis (EOR)
### Fabirc Modules
### System Controller
### Transceivers
- `show int e1/1 transceiver detail`
### PSU/Fan Trays
----
## Hardware Forwarding
### L3 mis-programming
### L2 mis-programming
### MAC address sync
----
## L1
### Ports Flapping
- `show int e1/1`
- `show int e1/1 transceiver detail`
### Ports not coming up
### Auto negotiation
### Interface/Port Errors
- `show interface counters errors `
- `show int eth 1/12 counters errors`
### POE
----
## FEX
### FEX
- `show fex`
- `show fex detail`
- `show interface port-channel 103 fex-intf`
- `reload fex <number>`
----
## Performance Issues
### Packet Drops
### Oversubscription
### High CPU
### Software Switching
### MTU
----
## L2 Protocols
- `show int status`
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`
- `show mac address-table`
- `show hardware mac address-table 1 dynamic`
- `show ip arp detail`
- `show ip arp detail | include x.x.x.x`
### Rapid STP
- `show spanning-tree`
- `show spanning-tree brief`
- `show spanning-tree summary`
- `show spanning-tree vlan 1` 
### MST
### VLANs and Trunks
- `show vlan`
- `show vlan bri`
- `show vlan id 1`
- `show interface trunk`
- `show interface trunk vlan 1`
### Port-channels
- `sh port-channel summary`
- `show run interface port-channel 103 membership`
### VPC
- `show run vpc`
- `show vpc`
- `show vpc consistency global`
- `sh port-channel summary`
### Fabricpath
### VTP
### Private VLANs
### Dot1q/QinQ Tunnels
### LACP
### UDLD
----
## L3 Unicast Protocols
- `show ip int bri`
- `show ip route vrf all`
- `show ip route summary vrf all`
- `show ip route x.x.x.x`
- `test consistency-checker forwarding`
- `show consistency-checker forwarding `
### Static Routing
### Prefix-list
- `show ip prefix-list`
### Route-map
- `show run | sec route-map`
- `show route-map`
- `show route-map WORD`
### Redistribution
### Route Leaking
### OSPF
- `show ip ospf`
- `show ospfv3`
- `show ip protocols`
- `show ip ospf 1 interface e1/1`
- `show ip ospf neighbor`
- `show ip ospf interface`
- `show ip ospf summary-address`
- `show ip ospf database`
### EIGRP
- `show ip eigrp topology x.x.x.x`
- `show ip protocols`
- `show ip eigrp topology detail-links vrf all`
### ISIS
### BGP
- `show run bgp`
- `show bgp all`
- `Show bgp vrf all all summary`
- `show bgp vrf all all neighbors`
- `show bgp sessions vrf all`
### DHCP
- `Show ip dhcp statistics`
### BFD
### PBR
### FHRP - HSRP
- `show hsrp`
- `show hsrp brief`
- `show hsrp summary`
- `show ip dhcp statistics`
- `show hsro all details`
### FHRP - VRRP
### VRF
### NAT
### RIP
### Tunnels GRE/IPIP/etc
----
## Multicast
### IGMP
- `sh ip igmp snooping vlan 2`
### PIM Sparse
- `show run pim`
- `show ip pim neighbor`
- `show ip mroute vrf all`
- `show ip mroute x.x.x.x`
- `sh ip pim internal vpc rpf-source `
- `show ip pim interface vlan 2 | i i DR`
### PIM Bidir
----
## VXLAN
- `sh nve interface nve 1 detail`
----
## MPLS
----
## QoS
### L3 QoS
### L2 QoS
----
## Management
### NTP
### PTP
----
## Monitoring
### SNMP
### Netflow
### RMON
### Flow Tables
### SFLOW
### Statistics
### SPAN
- `show hardware rate-limiter span` 
### VACL
### Telemetry
----
## Security
### RADIUS
### TACAS
### CTS
### MACsec
### Access lists
### User accounts/permissions
### storm control
----
## System Management
### TCAM Carving
### VDC
### Forwarding Mode/template
### CoPP
- `show policy-map interface control-plane`
### Hardware Rate Limiters
### Warp Mode
### NX-OS/upgrade/Downgrade
- `show upgrade history `
### Rollback/Checkpoint
### Config Sync/Switch Profile
### Generic System Management
### Proactive
### Health Check
### Device Unresponsive
### Unexpected Reboot
### Password Recovery
### Bootflash Issues
### Process Crash/Core
### Memory Leak
----
## Licensing
### Licensing
----
## FC/FCoE
----
## Automation/Programmability
### EEM
- `show event manager system-policy`
- `show event manager environment all`
- `show event manager event-types all`
-   
----
## DME Inconsistency
----
## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`
- `show accounting log`
- `show processes cpu history `
- `show switching-mode `
- `show systemr reset-reason`
----
## Packet Capture
## Packet Capture
### ELAM WRAPER
```
debug platform internal tah elam
trigger init
reset
set outer ipv4 dst_ip 172.29.88.251 src_ip 10.93.46.151
start
report
```
```
sh hardware internal tah niv_idx 0x605
```
```
PLNSWCS933601(TAH-elam-insel6)# report
HEAVENLY ELAM REPORT SUMMARY
slot - 1, asic - 0, slice - 0
============================

Incoming Interface: Eth1/31
Src Idx : 0x605, Src BD : 3101
Outgoing Interface Info: dmod 1, dpid 36
Dst Idx : 0x609, Dst BD : 4003

Packet Type: IPv4

Dst MAC address: A0:3D:6E:4F:F3:47
Src MAC address: 70:7D:B9:00:E1:3F
.1q Tag0 VLAN: 3101,  cos = 0x0

Dst IPv4 address: 172.29.88.251
Src IPv4 address: 10.93.46.151
Ver     =  4, DSCP    =    0, Don't Fragment = 0
Proto   =  1, TTL     =    7, More Fragments = 0
Hdr len = 20, Pkt len =   92, Checksum       = 0x568d

L4 Protocol  : 1
ICMP type    : 8
ICMP code    : 0

Drop Info:
----------

LUA:
LUB:
LUC:
LUD:
Final Drops:

vntag:
vntag_valid    : 0
vntag_vir      : 0
vntag_svif     : 0

ELAM not triggered yet on slot - 1, asic - 0, slice - 1

PLNSWCS933601(TAH-elam-insel6)#




PLNSWCS933601(config)# sh hardware internal tah niv_idx 0x605
niv_idx  : 1541
Interface: Ethernet1/31
PLNSWCS933601(config)# sh hardware internal tah niv_idx 0x609
niv_idx  : 1545
Interface: Ethernet1/28
PLNSWCS933601(config)#
```

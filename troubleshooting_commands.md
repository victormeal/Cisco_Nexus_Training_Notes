# Troubleshooting Commands
## Basic Cammands
- `show version`
- `show module`
- `show run`
- `show logg log`
- `show logg nvram`
- `show tech`

----
## Layer 2

### L2 Connectivity
- `show int status`
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`

### VLANs
- `show vlan bri`

### Port-channels
- `sh port-channel summary`

### Spanning-tree (STP)
- `sh spanning-tree brief`
- `sh spanning-tree summary `

### VPC
- `show vpc`
- `sh port-channel summary`
----
## L2/3
- `show ip arp detail`
- `show ip arp detail | include x.x.x.x`
----
## Layer 3
### L3 Connectivity
- `show ip int bri`
- `show ip route vrf all`
- `show ip route x.x.x.x`



### OSPF
- `show ospf`
- `show ospfv3`
- `show ip protocols`

### EIGRP
- `show ip eigrp topology x.x.x.x`
- `show ip protocols`

### BGP
- `Show ip bgp summary`
- `show ip bgp neighbors`
- 

### Multicast
- `show run pim`
- `show ip pim neighbor`
- `show ip mroute vrf all`
- `show ip mroute x.x.x.x`
- `sh ip igmp snooping vlan 2`
- `sh ip pim internal vpc rpf-source `
- `show ip pim interface vlan 2 | i i DR`
----
## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`


# Troubleshooting Commands
## Basic Cammands
- `show version`
- `show module`
- `show run`
- `show logg log`
- `show logg nvram`
- `show tech`
- `show tech details`
- `show feature | i i enable`
- `sh run | i i feature`

----
## Layer 1
### Hardware
- `show enviroment`
- `show inventory`
- `show module`
- `show hardware profile status`
- `show interface e1/1 capabilities`

### Layer 1 Connectivity
- `show int e1/1`
- `show int e1/1 tramsceiver detail`

### Interface errors
- `show interface counters errors `
- `show int eth 1/12 counters errors`

----
## Layer 2

### L2 Connectivity
- `show int status`
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`

### VLANs and Trunks
- `show vlan`
- `show vlan bri`
- `show interface trunk`
- `show interface trunk vlan 1`

### Port-channels
- `sh port-channel summary`

### Spanning-tree (STP)
- `show spanning-tree`
- `show spanning-tree brief`
- `show spanning-tree summary`
- `show spanning-tree vlan 1` 

### VPC
- `show run vpc`
- `show vpc`
- `show vpc consistency global`
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
- `show ip route summary vrf all`
- `show ip route x.x.x.x`
- `test consistency-checker forwarding`
- `show consistency-checker forwarding `

### Routing Protocols
#### OSPF
- `show ospf`
- `show ospfv3`
- `show ip protocols`

#### EIGRP
- `show ip eigrp topology x.x.x.x`
- `show ip protocols`

#### BGP
- `show run bgp`
- `show bgp all`
- `Show bgp vrf all all summary`
- `show bgp vrf all all neighbors`
- `show bgp sessions vrf all`

----
## IP Services
### Multicast
- `show run pim`
- `show ip pim neighbor`
- `show ip mroute vrf all`
- `show ip mroute x.x.x.x`
- `sh ip igmp snooping vlan 2`
- `sh ip pim internal vpc rpf-source `
- `show ip pim interface vlan 2 | i i DR`

### HSRP
- `show hsrp`
- `show hsrp summary`
- `show ip dhcp statistics`
- `show hsro all details`

### DHCP
- `Show ip dhcp statistics`

### COPP
- `show policy-map interface control-plane`

### Prefix-list
- `show ip prefix-list`

### Route-map
- `show run | sec route-map`
- `show route-map`
- `show route-map WORD`

----
## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`
- `show accounting log`
- `show processes cpu history `
- `show switching-mode `


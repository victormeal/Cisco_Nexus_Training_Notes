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

### FEX
- `show fex`
- `show fex detail`
- `show interface port-channel 103 fex-intf`

----
## Layer 2

### L2 Connectivity
- `show int status`
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`
- `show mac address-table`
- `show hardware mac address-table 1 dynamic`

### VLANs and Trunks
- `show vlan`
- `show vlan bri`
- `show vlan id 1`
- `show interface trunk`
- `show interface trunk vlan 1`

### Port-channels
- `sh port-channel summary`
- `show run interface port-channel 103 membership`

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

----
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


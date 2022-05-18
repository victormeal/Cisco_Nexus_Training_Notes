# Troubleshooting commands

- `show tech-support details > $(SWITCHNAME)-tech-details`

## Basic health check
- interface drops - queueing drops
- copp drops
- CPU utilization
- MTS buffers
- cores
- loggs (logging log and nvram)
- packet capture
- check MTU

## Hardware
- `show enviroment`
- `show inventory`
- `show module`
- `show hardware profile status`
- `show environment temperature `

## TCAM
- `show run all | i "tcam region"`
- `show hardware access-list resource utilization`


## CPU (High utilization)
- `show processes cpu history`
- `show processes cpu sort`
- check for cores, copp, mts, logs and packet caputre

### NX-OS/upgrade/Downgrade
- `show upgrade history`
- `show system reset-reason`

## Interfaces and ports
- `show int e1/1`
- `show int brief`
- `show int status`
- `show int e1/1 transceiver detail`
- `show interface e1/1 capabilities`
- `show int eth 1/12 counters errors`
- `clear counters interface e1/22 `
- `show interface counters errors `

## Packet drops
- `show hardware internal interface asic counters module 1`
- `clear hardware internal interface-all asic counters module 1`
- https://techzone.cisco.com/t5/Nexus-9300/Nexus-9200-931xx-Tahoe-ASIC-Drop-Counters-Description/ta-p/994494
- `show hardware internal tah buffer counter`


## Interfaces and ports flapping
- `show lacp internal event-history int <module / port number> | no-more`
- `show system internal ethpm event-history int <module / port number>  | no-more`
```
attach mod <>
show hardware internal tah event-history front-port <port number  | no-more
show system internal port-client event-history port <port number>  | no-more
```
- `attach mod 1`
  - `show hardware internal tah event-history front-port 1`
  - `show hardware internal tah link-events | no-more`

```
- show tech ethpm

- show tech ethpc
```
- `show system internal ethpm event-history inter e1/45 | b "Fri Oct 23 12:13:06" | i i "triggered event" prev 2 next 2`

## LACP
```
show lacp counters interface port-channel 10
show lacp neighbor interface port-channel 10
show lacp internal event-history interface e3/1
show system internal ethpm event-history interface port-channel 10
show port-channel internal event-history interface port-channel 10
```
## Port-channel
- `show port-channel load-balance`
- `show port-channel load-balance forwarding-path interface port-channel 37 src-ip 10.1.1.1 dst-ip 20.1.1.1`

## L2 Adjacency
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`
- `show mac address-table`
- `show hardware mac address-table 1 dynamic`
- `show ip arp detail`
- `show ip arp detail | include x.x.x.x`

## MAC Moves
- `show logging level l2fm`
- `show mac address-table notification mac-move`

## VLANs
- `show vlan`
- `show vlan bri`
- `show vlan id 1`
- `show interface trunk`
- `show interface trunk vlan 1`

## Spanning-tree
- `show spanning-tree detail | i occurr|from|exec`

## VTP
- `show vtp status`
- `clear vtp counters`
- `show vtp counters`
- `show vtp interface`
- `show vtp datafile`

## Private VLANs
- `show vlan private-vlan`
- `show interface switchport`
- `show interface private-vlan mapping`
- `show interface vlan primary-vlan-id private-vlan mapping`
- `show vlan counters`

## Port-channels
- `sh port-channel summary`
- `show run interface port-channel 103 membership`
- `show port-channel load-balance forwarding-path interface port-channel 1 dst-ip 1.1.1.1 src-ip 2.2.2.2 `

## Spanning-tree (STP)
- `show spanning-tree`
- `show spanning-tree brief`
- `show spanning-tree summary`
- `show spanning-tree vlan 1`

## vPC
- `show run vpc`
- `show vpc`
- `show vpc consistency-parameters global`
- `sh port-channel summary`

## FEX
- `show fex`
- `show fex detail`
- `show interface port-channel 103 fex-intf`
- `reload fex <number>`
- `sh system internal fex log`


## NTP
- `show ntp peers`
- `show ntp peer-status`
- `show ntp statistics peer ipaddr <x.x.x.x>` 
- `show ntp authentication-keys`
- `show ntp trusted-keys`
- `show ntp authentication-status`

## N7K - VDC
- `show vdc`
- `show vdc detail`
- `show vdc current-vdc`
- `show vdc membership`
- `show vdc resource`


- `switchto vdc <name>`
- `vdc <name>`  <<< vdc config

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/sw/nx-os/virtual_device_context/configuration/guide/b-7k-Cisco-Nexus-7000-Series-NX-OS-Virtual-Device-Context-Configuration-Guide/managing-vdc.html

## Prefix-list
- `show ip prefix-list`

## Route-map
- `show run | sec route-map`
- `show route-map`
- `show route-map WORD`

## CoPP
- `show copp status`
- `show policy-map interface control-plane`
- `show policy-map interface control-plane | i i class|drop`
- `show policy-map interface control-plane class <>`
- `show hardware rate-limiter`
- `sh int e1/1 counters detailed`
- `clear copp statistics`

### SPAN
- `show hardware rate-limiter span` 

## L3 - IP unicast protocols
- `show ip int bri`
- `show ip route vrf all`
- `show ip route summary vrf all`
- `show ip route x.x.x.x`
- `test consistency-checker forwarding`
- `show consistency-checker forwarding `
- `show hardware internal forwarding table utilization`

## OSPF
- `show ip ospf`
- `show ospfv3`
- `show ip protocols`
- `show ip ospf 1 interface e1/1`
- `show ip ospf neighbor`
- `show ip ospf interface`
- `show ip ospf summary-address`
- `show ip ospf database`
- `show ip ospf internal event-history hello`
- `show ip ospf internal event-history adjacency`
## EIGRP
- `show ip eigrp topology x.x.x.x`
- `show ip protocols`
- `show ip eigrp topology detail-links vrf all`

## ISIS
- `show run isis`
- `show isis adjacency`
- `show isis topology`
- `show isis database detail`

## BGP
- `show tech-support bgp detailed > $(SWITCHNAME)-tech-BGP`
- `show tech-support routing ip unicast > $(SWITCHNAME)-tech-routing`
- `show run bgp`
- `show ip bgp`
- `show ip bgp neighbors`
- `show ip bgp neighbors 10.58.144.33` 

- `show bgp all`
- `Show bgp vrf all all summary`
- `show bgp vrf all all neighbors`
- `show bgp sessions vrf all`

- `show ip bgp x.x.x.x/x vrf <>`
- `show ip bgp neighbors x.x.x.x vrf <>`

## DHCP
- `Show ip dhcp statistics`

## HSRP
- `show hsrp`
- `show hsrp brief`
- `show hsrp summary`
- `show ip dhcp statistics`
- `show hsrp all details`

## Multicast
### IGMP
- `show ip igmp groups 239.194.87.10`
- `show ip igmp interface vlan 305`
- `sh ip igmp snooping vlan 2`
- `clear ip igmp groups 239.194.87.10`
### PIM Sparse
- `show run pim`
- `show ip pim neighbor`
- `show ip mroute vrf all`
- `show ip mroute x.x.x.x`
- `show ip pim interface brief`
- `show ip pim internal event-history null-register | i 239.4.4.1`
- `show ip mroute x.x.x.x summary`
- `show ip mroute x.x.x.x summary software-forwarded`
- `sh ip pim internal vpc rpf-source `
- `show ip pim interface vlan 2 | i i DR`

## VXLAN
- `Show vxlan`
- `Show nve vni`
- `Show nve interface nve1 detail`
- `Show nve peers`
- `sh bgp l2vpn evpn summary`
- `h bgp l2vpn evpn neighbors 10.100.100.22 advertised-routes  | i 0035.1ac1.37c2 p 1 n 1`

### Flood n Learn with mutlicast
- `show ip pim rp`
- `show ip pim neig`
- `show nve peers`
- `show nve vni`

## EEM
- `show event manager system-policy`
- `show event manager environment all`
- `show event manager event-types all`

## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`
- `show accounting log`
- `show switching-mode `
- `show systemr reset-reason`
- `show system interal ethpm event-history interface e1/1`
- `show lacp internal event-history interface e1/1`
- `show hardware internal buffer pkt-stats`
- `sh system reset-reason `
- `sh system internal mts buffers details`
- `show system internal mts opcodes | i 86017`
- `show system internal mts sup sap <> description`
- `show system internal mts buffers summary `
- `show ip traffic`
- `show hardware internal forwarding table utilization`
- `show ip route summary vrf all`
- `show ip arp summary vrf all`
- `show forwarding summary vrf all`
- `show ip route vrf all`
- `show ip arp vrf all`
- `show forwarding vrf all`
- `show system internal forwarding vrf all route`
- `show diagnostic result module all detail`
- `diagnostic clear result module all`
- `diagnostic start module 1 test all`
- `show module internal exceptionlog`

```
attach mod 1
show hardware internal tah event-history front-port 43
show hardware internal port-client link event
show hardware internal tah link-events fp-port 51
```
## ELAM wrapper
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
sh system internal ethpm info all | i i dpid=48  
```
## ELAM
```
show hardware internal tah interface e1/1
attach module 1
debug platform internal tah elam asic <0>
trigger init asic <> slice <> lu-a2d 1
reset
set outer ipv4 dst_ip 172.29.88.251 src_ip 10.93.46.151
start
report
```
```
sh hardware internal tah niv_idx 0x605
```
```
sh system internal ethpm info all | i i dpid=48  
```

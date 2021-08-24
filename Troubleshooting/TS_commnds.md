# Troubleshooting commands

## Hardware
- `show enviroment`
- `show inventory`
- `show module`
- `show hardware profile status`
- `show environment temperature `

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
- `show interface counters errors `

### Interfaces and ports flapping
- `attach mod 1`
  - `show hardware internal tah event-history front-port 1`
  - `show hardware internal tah link-events | no-more`

## L2 Adjacency
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`
- `show mac address-table`
- `show hardware mac address-table 1 dynamic`
- `show ip arp detail`
- `show ip arp detail | include x.x.x.x`

## VLANs
- `show vlan`
- `show vlan bri`
- `show vlan id 1`
- `show interface trunk`
- `show interface trunk vlan 1`

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
- `show vpc consistency global`
- `sh port-channel summary`

## FEX
- `show fex`
- `show fex detail`
- `show interface port-channel 103 fex-intf`
- `reload fex <number>`

## Prefix-list
- `show ip prefix-list`

## Route-map
- `show run | sec route-map`
- `show route-map`
- `show route-map WORD`

### SPAN
- `show hardware rate-limiter span` 

## L3 - IP unicast protocols
- `show ip int bri`
- `show ip route vrf all`
- `show ip route summary vrf all`
- `show ip route x.x.x.x`
- `test consistency-checker forwarding`
- `show consistency-checker forwarding `

## OSPF
- `show ip ospf`
- `show ospfv3`
- `show ip protocols`
- `show ip ospf 1 interface e1/1`
- `show ip ospf neighbor`
- `show ip ospf interface`
- `show ip ospf summary-address`
- `show ip ospf database`
## EIGRP
- `show ip eigrp topology x.x.x.x`
- `show ip protocols`
- `show ip eigrp topology detail-links vrf all`

## BGP
- `show run bgp`
- `show ip bgp`
- `show ip bgp neighbors`

- `show bgp all`
- `Show bgp vrf all all summary`
- `show bgp vrf all all neighbors`
- `show bgp sessions vrf all`

## DHCP
- `Show ip dhcp statistics`

## HSRP
- `show hsrp`
- `show hsrp brief`
- `show hsrp summary`
- `show ip dhcp statistics`
- `show hsro all details`

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

## VXLAN
- `sh nve interface nve 1 detail`

## EEM
- `show event manager system-policy`
- `show event manager environment all`
- `show event manager event-types all`

## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`
- `show accounting log`
- `show processes cpu history `
- `show switching-mode `
- `show systemr reset-reason`
- `show system interal ethpm event-history interface e1/1`
- `show lacp internal event-history interface e1/1`
- `show hardware internal buffer pkt-stats`
- `sh system reset-reason `
- `sh system internal mts buffers details`

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
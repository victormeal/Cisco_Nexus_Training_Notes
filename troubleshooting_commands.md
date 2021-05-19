# Troubleshooting Commands
## Basic Cammands
- `show version`
- `show module`
- `show run`
- `show logg log`
- `show logg nvram`
- `show tech`

## L2 Connectivity
- `show int status`
- `show cdp neighbors`
- `show cdp neighbors interface port-channel X`

## L2/3
- `show ip arp detail`
- `show ip arp detail | include x.x.x.x`

## L3 Connectivity
- `show ip int bri`
- `show ip route vrf all`
- `show ip route x.x.x.x`

## VPC
- `show vpc`

## Multicast
- `show run pim`
- `show ip pim neighbors`
- `show ip mroute vrf all`
- `show ip mroute x.x.x.x`
- `show ip igmp snooping groups vlan X`

## Miscellaneous
- `sh hardware capacity forwarding | b TCAM`


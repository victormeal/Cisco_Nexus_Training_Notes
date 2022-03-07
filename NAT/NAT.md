# NAT
- We punt first packet to CPU to create the new nat translation.

## config example (dynamic NAT Overload with pool)
```
hardware access-list tcam region ing-l3-vlan-qos 256
hardware access-list tcam region nat 256

feature nat

ip access-list NAT-INSIDE
  10 permit ip 7.7.7.7/24 any 

ip nat pool NAU-65-173-245 65.173.245.250 65.173.245.250 prefix-length 32
ip nat translation timeout 13000
ip nat translation max-entries 300
ip nat inside source list NAT-INSIDE pool NAU-65-173-245 overload add-route 


interface Ethernet1/4
  ip nat inside 

interface Ethernet1/52
  ip nat outside
```
- overload add-route <<<<< Add a static route for outside local address
```
NAT_N9K(config)# show ip route 65.173.245.250
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

65.173.245.250/32, ubest/mbest: 1/0
    *via 7.7.7.7, [1/0], 00:12:48, nat
NAT_N9K(config)# 

```

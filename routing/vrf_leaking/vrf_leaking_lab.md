# VRF Leaking
----
```
MXC.TAC.L.04-93180YC-FX3S-02# sh run

feature ospf
feature bgp
feature eigrp
feature interface-vlan

vlan 1-2

ip prefix-list myPList seq 5 permit 192.168.2.0/24 
ip prefix-list myPList2 seq 5 permit 1.1.1.0/24 
route-map myMAP permit 10
  match ip address prefix-list myPList 
route-map myMAP2 permit 10
  match ip address prefix-list myPList2 
vrf context BLUE
  address-family ipv4 unicast
    import vrf default map myMAP2
    export vrf default map myMAP
vrf context RED

interface Vlan1
  no shutdown
  ip address 1.1.1.2/24
  ip router ospf 1 area 0.0.0.0

interface Vlan2
  no shutdown
  vrf member BLUE
  ip address 2.2.2.1/24
  ip router eigrp 1


interface Ethernet1/44
  switchport
  switchport mode trunk
  no shutdown

interface Ethernet1/45
  switchport
  switchport mode trunk
  no shutdown

router eigrp 1
  router-id 0.0.0.3
  vrf BLUE
    default-information originate always
    
router ospf 1
  router-id 0.0.0.3
  
router bgp 1
  address-family ipv4 unicast
    redistribute direct route-map myMAP2
    
  vrf BLUE
    address-family ipv4 unicast
      redistribute eigrp 1 route-map myMAP
```
----
```
MXC.TAC.L.04-93180YC-FX3S-02# sh ip route vrf default 
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

1.1.1.0/24, ubest/mbest: 1/0, attached
    *via 1.1.1.2, Vlan1, [0/0], 01:28:53, direct
1.1.1.2/32, ubest/mbest: 1/0, attached
    *via 1.1.1.2, Vlan1, [0/0], 01:28:53, local
192.168.1.1/32, ubest/mbest: 1/0
    *via 1.1.1.1, Vlan1, [110/41], 01:25:54, ospf-1, inter
192.168.2.0/24, ubest/mbest: 1/0
    *via 2.2.2.2%BLUE, Vlan2, [20/130816], 00:10:45, bgp-1, external, tag 1

MXC.TAC.L.04-93180YC-FX3S-02# 
```
----
```
MXC.TAC.L.04-93180YC-FX3S-02# sh ip route vrf BLUE
IP Route Table for VRF "BLUE"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

1.1.1.0/24, ubest/mbest: 1/0, attached
    *via 1.1.1.2%default, Vlan1, [20/0], 00:09:29, bgp-1, external, tag 1
2.2.2.0/24, ubest/mbest: 1/0, attached
    *via 2.2.2.1, Vlan2, [0/0], 00:55:29, direct
2.2.2.1/32, ubest/mbest: 1/0, attached
    *via 2.2.2.1, Vlan2, [0/0], 00:55:29, local
192.168.2.0/24, ubest/mbest: 1/0
    *via 2.2.2.2, Vlan2, [90/130816], 00:48:17, eigrp-1, internal

MXC.TAC.L.04-93180YC-FX3S-02# 
```
----
```
MXC.TAC.L.04-93180YC-FX3S-02# ping 192.168.2.2 vrf default 
PING 192.168.2.2 (192.168.2.2): 56 data bytes
64 bytes from 192.168.2.2: icmp_seq=0 ttl=254 time=1.121 ms
64 bytes from 192.168.2.2: icmp_seq=1 ttl=254 time=0.936 ms
64 bytes from 192.168.2.2: icmp_seq=2 ttl=254 time=0.63 ms
64 bytes from 192.168.2.2: icmp_seq=3 ttl=254 time=0.758 ms
64 bytes from 192.168.2.2: icmp_seq=4 ttl=254 time=0.705 ms

--- 192.168.2.2 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 0.63/0.829/1.121 ms
MXC.TAC.L.04-93180YC-FX3S-02# 
```

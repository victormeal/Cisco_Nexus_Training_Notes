# Spine
```
SPINE_01(config)# sh run

hostname SPINE_01

feature ospf
feature pim
feature interface-vlan

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1

interface Vlan1

interface Ethernet1/1
  ip address 192.168.1.1/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/2
  ip address 192.168.2.1/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface loopback0
  ip address 7.7.7.7/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
 
router ospf 1

SPINE_01(config)# 
```
----
# LeafA
```
Leaf_A# sh run

hostname Leaf_A

feature ospf
feature pim
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 5000

interface Vlan1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 5000 mcast-group 239.1.1.1

interface Ethernet1/1
  ip address 192.168.1.2/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/46
  switchport
  switchport access vlan 10
  no shutdown


interface loopback0
  ip address 11.11.11.11/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

router ospf 1

Leaf_A#
```

----
# LeafB
```
Leaf_B# sh run

hostname Leaf_B

feature ospf
feature pim
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 5000

interface Vlan1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 5000 mcast-group 239.1.1.1

interface Ethernet1/1
  ip address 192.168.2.2/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/45
  switchport
  switchport access vlan 10
  no shutdown

interface loopback0
  ip address 22.22.22.22/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

router ospf 1


Leaf_B# 
```

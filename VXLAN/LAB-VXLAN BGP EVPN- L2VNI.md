# VXLAN BGP EVPN- L2VNI

## SPINE
```
SPINE# sh run

hostname SPINE

nv overlay evpn
feature ospf
feature bgp
feature pim
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
ip pim anycast-rp 1.2.3.4 192.168.0.1
vlan 1

interface Vlan1

interface Ethernet1/1
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/3
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface loopback0
  ip address 192.168.0.1/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

interface loopback1
  ip address 1.2.3.4/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

router ospf 1
  log-adjacency-changes
router bgp 64520
  log-neighbor-changes
  address-family ipv4 unicast
  address-family l2vpn evpn
    retain route-target all
  template peer VXLAN_LEAF
    remote-as 64520
    update-source loopback0
    address-family ipv4 unicast
      send-community extended
      route-reflector-client
      soft-reconfiguration inbound
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 192.168.0.3
    inherit peer VXLAN_LEAF
  neighbor 192.168.0.5
    inherit peer VXLAN_LEAF

SPINE# 
```
----
## Leaf1
```
Leaf1# sh run

hostname Leaf1

nv overlay evpn
feature ospf
feature bgp
feature pim
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

fabric forwarding anycast-gateway-mac 0000.0011.1234
ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 100010

interface Vlan1

interface Vlan10
  no shutdown
  ip address 10.10.1.254/24
  fabric forwarding mode anycast-gateway

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback0
  member vni 100010
    suppress-arp
    mcast-group 224.1.1.192

interface Ethernet1/1
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/44
  switchport
  switchport access vlan 10
  no shutdown

interface loopback0
  ip address 192.168.0.3/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
 
router ospf 1
  log-adjacency-changes
router bgp 64520
  log-neighbor-changes
  address-family ipv4 unicast
  address-family l2vpn evpn
  template peer VXLAN_SPINE
    remote-as 64520
    update-source loopback0
    address-family ipv4 unicast
      send-community extended
      soft-reconfiguration inbound
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 192.168.0.1
    inherit peer VXLAN_SPINE
evpn
  vni 100010 l2
    rd auto
    route-target import auto
    route-target export auto

Leaf1# 
```
----
## Leaf2
```
Leaf2# sh run

hostname Leaf2

nv overlay evpn
feature ospf
feature bgp
feature pim
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

fabric forwarding anycast-gateway-mac 0000.0011.1234
ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 100010

interface Vlan1

interface Vlan10
  no shutdown
  ip address 10.10.1.254/24
  fabric forwarding mode anycast-gateway

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback0
  member vni 100010
    suppress-arp
    mcast-group 224.1.1.192

interface Ethernet1/1
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/46
  switchport
  switchport access vlan 10
  no shutdown

interface loopback0
  ip address 192.168.0.5/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
 
router ospf 1
  log-adjacency-changes
router bgp 64520
  log-neighbor-changes
  address-family ipv4 unicast
  address-family l2vpn evpn
  template peer VXLAN_SPINE
    remote-as 64520
    update-source loopback0
    address-family ipv4 unicast
      send-community extended
      soft-reconfiguration inbound
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 192.168.0.1
    inherit peer VXLAN_SPINE
evpn
  vni 100010 l2
    rd auto
    route-target import auto
    route-target export auto

Leaf2# 
```
----
## Host 1
```
Host1# sh run

hostname Host1

vlan 1

interface Ethernet1/44
  ip address 10.10.1.1/24
  no shutdown

Host1# 
```
----
## Host 2
```
Host2# sh run

hostname Host2

vlan 1

interface Ethernet1/46
  ip address 10.10.1.2/24
  no shutdown

Host2# 
```

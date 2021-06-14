# VXLAN
----

## L2VNI only - Config

----
## L2VNI and L3 VNI Config

### SPINE
```
version 9.3(7) Bios:version 05.43 
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
```
----
### LEAF1
```
version 9.3(7) Bios:version 01.03 
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

vlan 1,10,20,999
vlan 10
  vn-segment 100010
vlan 20
  vn-segment 100020
vlan 999
  vn-segment 100999

vrf context TENANT1
  vni 100999
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn

interface Vlan1

interface Vlan10
  no shutdown
  vrf member TENANT1
  ip address 10.10.1.254/24
  fabric forwarding mode anycast-gateway

interface Vlan20
  no shutdown
  vrf member TENANT1
  ip address 10.20.1.254/24
  fabric forwarding mode anycast-gateway

interface Vlan999
  no shutdown
  vrf member TENANT1
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback0
  member vni 100010
    suppress-arp
    mcast-group 224.1.1.192
  member vni 100020
    suppress-arp
    mcast-group 224.1.2.192
  member vni 100999 associate-vrf

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

interface Ethernet1/47
  switchport
  switchport access vlan 20
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
  vrf TENANT1
    log-neighbor-changes
    address-family ipv4 unicast
      network 10.10.1.0/24
      network 10.20.1.0/24
      advertise l2vpn evpn
evpn
  vni 100010 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 100020 l2
    rd auto
    route-target import auto
    route-target export auto
```
----
### LEAF2
```
version 9.3(7) Bios:version 01.03 
hostname Leaf2
vdc Leaf2 id 1
  limit-resource vlan minimum 16 maximum 4094
  limit-resource vrf minimum 2 maximum 4096
  limit-resource port-channel minimum 0 maximum 511
  limit-resource u4route-mem minimum 248 maximum 248
  limit-resource u6route-mem minimum 96 maximum 96
  limit-resource m4route-mem minimum 58 maximum 58
  limit-resource m6route-mem minimum 8 maximum 8
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

vlan 1,10,20,999
vlan 10
  vn-segment 100010
vlan 20
  vn-segment 100020
vlan 999
  vn-segment 100999

vrf context TENANT1
  vni 100999
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn

interface Vlan1

interface Vlan10
  no shutdown
  vrf member TENANT1
  ip address 10.10.1.254/24
  fabric forwarding mode anycast-gateway

interface Vlan20
  no shutdown
  vrf member TENANT1
  ip address 10.20.1.254/24
  fabric forwarding mode anycast-gateway

interface Vlan999
  no shutdown
  vrf member TENANT1
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback0
  member vni 100010
    suppress-arp
    mcast-group 224.1.1.192
  member vni 100020
    suppress-arp
    mcast-group 224.1.2.192
  member vni 100999 associate-vrf

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

interface Ethernet1/48
  switchport
  switchport access vlan 20
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
  vrf TENANT1
    log-neighbor-changes
    address-family ipv4 unicast
      network 10.10.1.0/24
      network 10.20.1.0/24
      advertise l2vpn evpn

evpn
  vni 100010 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 100020 l2
    rd auto
    route-target import auto
    route-target export auto
```
----
## HOST1 (vlan 10)
```
hostname Host1

ip route 0.0.0.0/0 10.10.1.254
vlan 1

interface Ethernet1/44
  ip address 10.10.1.1/24
  no shutdown

```
----
### HOST2 (vlan 10)
```
hostname Host2

ip route 0.0.0.0/0 10.10.1.254
vlan 1

interface Ethernet1/46
  ip address 10.10.1.2/24
  no shutdown
```
----
### HOST3 (vlan 20)
```
hostname Host3

ip route 0.0.0.0/0 10.20.1.254
vlan 1

interface Ethernet1/44
  ip address 10.20.1.1/24
  no shutdown
```
----
## HOST4 (vlan 20)
```
hostname Host4

ip route 0.0.0.0/0 10.20.1.254
vlan 1

interface Ethernet1/46
  ip address 10.20.1.2/24
  no shutdown
```

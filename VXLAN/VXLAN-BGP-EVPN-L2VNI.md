# VXLAN BGP EVPN- L2VNI
- https://www.youtube.com/watch?v=faUd0vcRzI8
## Spine (N1)
```
host N1

interface e1/1-4
no sw
mtu 9216
no shut

feature ospf

router ospf OSPF_UNDERLAY_NET
log-adjancency-changes

int lo0
description loopback
ip add 192.168.0.1/32
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0

int e1/1-4
medium p2p
ip unnumbered loopback0
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0
```
```
feature pim

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
ip pim anycast-rp 1.2.3.4 192.168.0.1
ip pim anycast-rp 1.2.3.4 192.168.0.2

int lo1
ip address 1.2.3.4/32
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0
ip pim sparse-mode

int lo0
ip pim sparse-mode

int e1/1-4
ip pim sparse-mode
```
```
nv overlay evpn
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
```
```
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
!neighbor 192.168.0.4
!inherit peer VXLAN_LEAF
neighbor 192.168.0.5
inherit peer VXLAN_LEAF
!neighbor 192.168.0.6
!inherit peer VXLAN_LEAF


```
----

## Leaf1 (N3)
```
host N3

int e1/1-2
no sw
mtu 9216
no shut

feature ospf

router ospf OSPF_UNDERLAY_NET
log-adjacency-changes

int e1/1-2
medium p2p
ip unnumbered lo0
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0

int lo0
description loopback
ip address 192.168.0.3/32
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0
```
```
feature pim

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8


int lo0
ip pim sparse-mode

int e1/1-2
ip pim sparse-mode
```
```
nv overlay evpn
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
```
```
int nve 1
no shut
host-reachability protocol bgp
source-interface loopback0
```
```
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
!neighbor 192.168.0.1
!inherit peer VXLAN_SPINE
```
```
hardware access-list tcam region arp-ether 256
```
```
fabric forwarding anycast-gateway-mac 0000.0011.1234
```
```
vlan 10
vn-segment 100010
```
```
int vlan 10
no shut
ip address 10.10.1.254/24
fabric forwarding mode anycast-gateway

int nve1
member vni 100010
suppress-arp
mcast-group 224.1.1.192

evpn
vni 100010 l2
rd auto
route-target import auto
route-target export auto
```
```
int e1/7
sw mode access
sw access vlan 10
no shut
```
----

## Leaf2 (N5)
```
host N5

int e1/3-4
no sw
mtu 9216
no shut

feature ospf

router ospf OSPF_UNDERLAY_NET
log-adjacency-changes

int e1/3-4
medium p2p
ip unnumbered lo0
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0

int lo0
description loopback
ip address 192.168.0.5/32
ip router ospf OSPF_UNDERLAY_NET area 0.0.0.0
```
```
feature pim

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8


int lo0
ip pim sparse-mode

int e1/1-2
ip pim sparse-mode
```
```
nv overlay evpn
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay 
```
```
int nve 1
no shut
host-reachability protocol bgp
source-interface loopback0
```
```
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
!neighbor 192.168.0.1
!inherit peer VXLAN_SPINE
```
```
hardware access-list tcam region arp-ether 256
```
```
fabric forwarding anycast-gateway-mac 0000.0011.1234
```
```
vlan 10
vn-segment 100010
```
```
int vlan 10
no shut
ip address 10.10.1.254/24
fabric forwarding mode anycast-gateway

int nve1
member vni 100010
suppress-arp
mcast-group 224.1.1.192

evpn
vni 100010 l2
rd auto
route-target import auto
route-target export auto
```
```
int e1/7
sw mode access
sw access vlan 10
no shut
```
----



## show commands
```
show ip ospf nei
show ip pim int br
show ip pim rp
show int nve 1
show ip bgp neighbors 192.168.0.1 | inc "Address family L2VPN EVPN"   -> en los leafs
show bgp levpn evpn summary
show fabric forwarding internal topo-info | grep Anycast   -> ??????
show bgp l2vpn evpn vni-id 100010
```


----
## Hosts
host 1 -> ip 10.10.1.1/24 10.10.1.254
host 3 -> ip 10.10.1.2/24 10.10.1.254

----
# VXLAN BGP EVPN- L3VNI
- https://www.youtube.com/watch?v=pu21qr3b1GA
## Leaf1 (N3)
```
vlan 999
vn-segment 100999
```
```
vrf context TENANT1
vni 100999
rd auto
address-family ipv4 unicast
route-target both auto
route-target both auto evpn
```
```
int vlan999
no shut
vrf member TENANT1
ip forward
```
```
int nve1
member vni 100999 associate-vrf

router bgp 64520
vrf TENANT1
log-neighbor-changes
address-family ipv4 unicast
network 10.10.1.0/24
advertise l2vpn evpn
```
```
! se borra la config al asociarlo a una vrf
int vlan10
vrf member TENANT1
ip address 10.10.1.254/24
fabric forwarding mode anycast-gateway
no shut
```
----
## Leaf2 (N5)
```
vlan 999
vn-segment 100999
```
```
vrf context TENANT1
vni 100999
rd auto
address-family ipv4 unicast
route-target both auto
route-target both auto evpn
```
```
int vlan999
no shut
vrf member TENANT1
ip forward
```
```
int nve1
member vni 100999 associate-vrf

router bgp 64520
vrf TENANT1
log-neighbor-changes
address-family ipv4 unicast
network 10.10.1.0/24
advertise l2vpn evpn
```
```
! se borra la config al asociarlo a una vrf
int vlan10
vrf member TENANT1
ip address 10.10.1.254/24
fabric forwarding mode anycast-gateway
no shut
```

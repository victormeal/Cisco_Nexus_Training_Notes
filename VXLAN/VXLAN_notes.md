# VXLAN

- Extension de L2 atraves de un underlay de L3
- extension de VLANS (12 bits = 4096) -> VXLAN Network Identifier (VNI) (24 bits)
- Spine n Leafs Topology (L3 connection between them)
- VTEP (Leafs) -> entrada al tunnel a otro Leaf
- Underlay L3 -> OSPF or ISIS only
- tunneles por BGP
- Tunneles dinamicos, no se crean hasta que se genera trafico
- (BUUM traffic-> broadcast,unknown unicast,multicast) flooding L2 -> multicast
- multicast solo para formar el tunnel, cuando manda trafico de flooding lo manda como un unicast encapsulado para cada uno
- with multicast vs ingress replication
- ingres replication manda mensajes a todos los peers como unicast, incluso a los que no quieren
- BGP EVPN -> interface nve

- L3 vni -> inter vlan routing (SVI 1, SVI 2)

- en VPC solo uno desencapsula, pero ambos encapsulan

- VTEP es una interfaz

## VXLAN Packet Structure

Outer MAC Header, Outer IP Header, Outer UDP Header, VXLAN Header, Original Layer 2 frame

UDP Port 4789

----
## Config
### Bridging Config (using Multicast)(Data plane learning)
```
feature ospf
feature pim
feature nv overlay
feature vn-segment-vlan-based
```
```
router ospf 10
```
```
vlan 1000
vn-segment 5000
```
```
system jumbomtu 9216
system routing template-vxlan-scale
! changing the routing template needs a reboot

copy r s
reload
y
```
```
int e1/49
no sw
ip add x.x.x.x/30
ip router ospf 10 area 0
ip pim sparse-mode
no shut
```
```
int e1/1
sw
sw access vlan 1000
no shut
```
```
int lo0  ---> RP
ip address 1.1.1.1/32
ip router ospf 10 area 0
ip pim sparse-mode

int lo1   ---> RP anycast, esta ip va ser igual en ambos switches
ip add 3.3.3.3/32
ip router ospf 10 area 0
ip pim sparse-mode

ip pim rp-address 3.3.3.3 group-list 224.0.0.0/4
ip pim anycast-rp 3.3.3.3 1.1.1.1
ip pim anycast-rp 3.3.3.3 2.2.2.2
```
```
interface nve1
no shut
source-interface loopback 0
member vni 5000 mcast-group 230.1.1.1
```
```
show nve interface

show nve peers. ---> puede estar vacia si no hay trafico
show nve vni
```
----
### BGP EVPN Config (using Ingress Replication)(Control Plane Learning)
#### Config the underlay
 - Routed Link
 - MTU Size  
```
feature ospf
system jumbomtu 9216
router ospf 10
```
```
int e1/49
no sw
ip add 10.10.10.1/30
ip router ospf 10 area 0
no shut
```
```
int lo0
ip add 1.1.1.1/32
ip router ospf 10 area 0
```
#### Config the overlay
 - Overlay Features
 - Anycast Gateway vMAC
 - BGP 
```
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn
```
```
fabric forwarding anycast-gateway-mac 0000.2222.3333  ---> MAC libre de escojer
```
```
int nve1
no shut
source-interface lo0
```
```
router bgp 65535
router-id 1.1.1.1
neighbor 2.2.2.2
remote-as 65535
update-source lo0
address-family l2vpn evpn
send-community
send-community extended
```
#### The First Tenant
- L3VNI
- VRF
- RD's and RT's
- VTEPs
- ARP Suppression
- Head-end Replication
- Anycast Gateway
- EVPN
```
vlan 101
vn-segment 900001
```
```
vrf context Tenant-1
vni 900001
rd auto
address-family ipv4 unicast
route-target both auto
route-target both auto evpn
```
```
in vlan 101
no shut
vrf member Tenant-1
ip forward
```
```
int nve1
member vni 5000
suppress-arp
ingress-replication protocol bgp
member vni 900001 associate-vrf
```
```
router bgp 65535
vrf Tenant-1
address-family ipv4 unicast
advertise l2vpn evpn
```
```
vlan 1000
vn-segment 5000
```
```
int vlan 1000
no shut
vrf member Tenant-1
ip address 192.168.0.1/24.  ----> esto ip va repetida en el otro, por ser anycast gateway
fabric forwarding mode anycast-gateway
```
```
evpn
vni 5000 l2
rd auto
route-target import auto
route-target export auto
```
```
int e1/1
sw
sw access vlan 1000
no shut
```
Agregar otro????
```
vlan 1001
vn-segment 5005
```
```
int vlan 1001
no shut
vrf member Tenant-1
ip address 192.168.10.1/24
fabric forwarding mode anycast-gateway
```
```
int nve1
member vni 5005
suppress-arp
ingress-replication protocol bgp
```
```
evpn
vni 5005 l2
rd auto
route-target import auto
route-target export auto
```
#### The Second Tenant
```
vrf context Tenant-2
vni 900002
rd auto
address-family ipv4 unicast
route-target both auto
route-target both auto evpn
```
```
vlan 102
vn-segment 900002
```
```
int vlan 102
no shut
vrf member Tenant-2
ip forward
```
```
vlan 900
vn-segment 6000
```
```
int vlan 900
no shut
vrf member Tenant-2
ip address 192.168.0.1/24
fabric forwarding mode anycast-gateway
```
```
int e1/2
sw
sw access vlan 900
no shut
```
```
int nve1
member vni 900002 associate-vrf
member vni 6000
suppress-arp
ingress-replication protocol bgp
```
```
evpn
vni 6000 l2
rd auto
route-target import auto
route-taget export auto
```
### Verfication
```
show bgp l2vpn evpn summary
show nve peers
show nve vni

show ip arp suppression-cache detail

show vxlan

show l2route evpn mac all
show l2route evpn mac-ip all
show bgp l2vpn evpn
```

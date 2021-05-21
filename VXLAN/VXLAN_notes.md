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

## VXLAN Packet Structure

Outer MAC Header, Outer IP Header, Outer UDP Header, VXLAN Header, Original Layer 2 frame

UDP Port 4789

----
## Config
### Bridging Config
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
### BGP EVPN Config

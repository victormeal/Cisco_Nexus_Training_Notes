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

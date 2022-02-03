# OTV

Encapsulation: Eth over MPLS over GRE over ipv4 unicast/multicast

## OTV Terminology
- OTV Edge Device
  - Device running OTV feature 
- AED (Authoritative Edge Device)
  - To have redudancy (odd vlans forwrd by one device, even vlans forward by another device)
- Extend VLANs
  - VLAN being bridged over OTV 
- Site VLAN
  - Internal VLAN used to elect AED 
- Site Identifier
  - To prevent loops, unique ID per DC, shared between AED
- Overlay Interface 
  - Logial OTV tunnel interface
- OTV Join Interface
  - Physical link or port-channel or loopback to route upstream to the DCI (Data Center Interconnect)  
- OTV Control Group
  - Multicast address used to discover the remote sites in the control plane
- OTV Data Group
  - Used when tunneling multicast traffic over OTV in the data plane
- OTV Adjaceny Server
  - by defaul OTV uses multicast, but with Adjaceny servercan remove this requirement by using unicast ¨Head En Replication¨  

----

OTV uses ISIS for the contol plane, for MAC learning

----

# OTV

## Terminology

- OTV Edge Device: Edge router running OTV
- Authoritative Edge Device (AED): Active forwarder when we have multiple Edge devices.
- Extend VLAN: The VLAN which will be bridged over OTV.
- Site VLAN: Internal VLAN which is used for AED election. Local significant.
- Site Identifier: Used for loop prevention. Unique ID per site (DC), shared between AED.
- Overlay interface: logical OTV
- OTV join interface: Physical link or Port-channel that you use for routing upstream towards the DCI. It cant be SVI. Is the L3 interface towards the DCI. We need to configure jumbo frames. Fragmentation is not possible.
- OTV Control group: Multicast IP which is used for discovering other OTV Sites in the control plane.
- OTV Data group: It is used for tunneling multicast traffic over OTV in data plane.
- ISIS: will be used for advertising the MAC addresses in OTV.

# Config

```
feature otv

interface e3/1
no shut
ip add 10.1.1.1/24
ip igmp version 3
mtu 9216

int e3/4
no shut
sw
sw mode trunk

vlan 11,99

otv site-vlan 99
otv site-identifier 111.111.111

interface overlay 1
otv join-interface e3/1
otv control-group 224.1.1.1
otv data-group 232.1.1.0/24
otv extend-vlan 11
no shut

```

```
show run otv
show otv
show otv isis adjacency
show otv isis database
show mac address-table

show otv adjacency detail
show otv vlan authoritative detail
show otv site detail
```


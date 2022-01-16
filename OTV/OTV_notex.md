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

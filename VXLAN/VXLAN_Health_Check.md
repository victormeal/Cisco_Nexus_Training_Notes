# VXLAN Health Check

+ IGP peering from Leaf to Spines.
       - `show ip ospf neighbors`
       
+ BGP EVPN peering from Leaf to Spines.
       - `show bgp l2vpn evpn summary`
       
+ NVE interface is up.
+ Check if VTEP is standalone or on vPC. On vPC both VTEPs should have NVE loopback with different primary IP and same secondary IP.
       - `show interface nve 1`

+ Make sure that the VNI state is up.
+ Check if the vlan is up.
+ This command is useful. You can check the VNI, if it is using multicast or ingress replication and if it is a L2 or L3 VNI.
       - `show nve vni`
       - `show nve vni 31800 detail`
       - `show vrf`


+ Check NVE peering. NVE peering are not always up.
       - Show nve peers
       - Show bgp l2 evpn | i <VTEP IP>
       - Show nve peers detail
       - Show nve internal peer-history peer x.x.x.x
       - Show nve internal platform interface detail

+ Check if Host MAC is locally learned
       - Show mac add vlan x
       - Show spanning-tree vlan x 

+ Check if Host MAC is remotely learned
       - Show mac add vlan x
+ When a VTEP learned remote MAC and NVE peer, your BGP EVPN Fabric control plane is healthy.

+ Check NVE L@VNI counter for ARP resolution
       - Show nve vni 31800 counters


Diapositive 95

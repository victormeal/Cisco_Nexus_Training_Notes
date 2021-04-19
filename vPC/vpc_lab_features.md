# vPC LAB
## Topology
???
## Basic Config (without features)

### Nexus_E

### Nexus_F

### Nexus_G

### Nexus_H
Basic Config
```
config t

feature vpc
feature lacp
feature interface-vlan

vlan 10
int vlan 10
ip add 10.10.10.10/24
no sh
exit

vpc domain 1
peer-keepalive destination 10.88.174.6 source 10.88.174.5 vrf management
role priority 100
exit

!peer-link
int e1/5
channel-group 10 mode active
no sh
exit
!
int po 10
sw
sw mode trunk
sw trunk allowed vlan 10
vpc peer-link
! bridge assurance enable by default or...
!spann port type network
no sh
```
Check vpc
```
nexus_H(config)# sh vpc
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 1   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success 
Per-vlan consistency status       : success                       
Type-2 consistency status         : failed  
Type-2 inconsistency reason       : QoSMgr Network QoS configuration incompatibl
e
vPC role                          : primary                       
Number of vPCs configured         : 0   
Peer Gateway                      : Disabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po10   up     10                                                          
         
nexus_H(config)# 
```
Adding a device (Nexus_J)
```
 config t
    int e1/6
    channel-group 11 mode active
    exit
    int po 11
    sw
      sw mode access
      sw access vlan 10
      vpc 11
      no sh
```

### Nexus_I
Basic Config
```
config t

feature vpc
feature lacp
feature interface-vlan

vlan 10
int vlan 10
ip add 10.10.10.20/24
no sh
exit

vpc domain 1
peer-keepalive destination 10.88.174.5 source 10.88.174.6 vrf management
role priority 110
exit

!peer-link
int e1/8
channel-group 10 mode active
no sh
exit
!
int po 10
sw
sw mode trunk
sw trunk allowed vlan 10
vpc peer-link
! bridge assurance enable by default or...
!spann port type network
no sh
```
Check vpc
```
nexus_I# sh vpc
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 1   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success 
Per-vlan consistency status       : success                       
Type-2 consistency status         : failed  
Type-2 inconsistency reason       : QoSMgr Network QoS configuration incompatibl
e
vPC role                          : secondary                     
Number of vPCs configured         : 0   
Peer Gateway                      : Disabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po10   up     10                                                          
         
nexus_I#
```
Adding a device (Nexus_J)
```
 config t
    int e1/6
    channel-group 11 mode active
    exit
    int po 11
    sw
      sw mode access
      sw access vlan 10
      vpc 11
      no sh
```

### Nexus_J
Basic Config (creating port-channel)
```
config t
feature lacp
feature interface-vlan

vlan 10
int vlan 10
ip add 10.10.10.30/24
no sh

int e1/5, e1/8
channel-group 11 mode active
! channel-group 11 force mode active
no sh
int po 11
sw
sw mode access
sw access vlan 10
no sh
```
Check port-channel
```
nexus_J# sh port-channel sum
Flags:  D - Down        P - Up in port-channel (members)
        I - Individual  H - Hot-standby (LACP only)
        s - Suspended   r - Module-removed
        b - BFD Session Wait
        S - Switched    R - Routed
        U - Up (port-channel)
        p - Up in delay-lacp mode (member)
        M - Not in use. Min-links not met
--------------------------------------------------------------------------------
Group Port-       Type     Protocol  Member Ports
      Channel
--------------------------------------------------------------------------------
11    Po11(SU)    Eth      LACP      Eth1/6(P)    Eth1/8(P)    
nexus_J# 
```
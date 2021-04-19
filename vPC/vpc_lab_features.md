# vPC LAB
 ## vPC Features vPC 
- **Peer-switch**: allows a pair of vPC peer devices to appear as a single Spanning Tree 
Protocol root in the Layer 2 topology (they have the same bridge ID). vPC peer-switch must be configured on both 
vPC peer devices to become operational.
        - avoid STP problems
        - both switches send BPDUs
        - Nexus needs to be the root
```
 config t
 vpc domain ?
 peer-switch
```
- **Bridge Assurance**: causes the switch to send BPDUs on all operational ports that carry a STP port type setting of 
"network", including alternate and backup ports for each hello time period. If a neighbor port stops receiving 
BPDUs, the port is moved into the blocking state. If the blocked port begins receiving BPDUs again, it is removed 
from bridge assurance blocking state, and goes through normal Rapid-PVST transition.  

- **PeerGateway**: allows a vPC peer device to act as the active gateway for packets addressed to the other peer 
device router MAC. It keeps the forwarding of traffic local to the vPC peer device and avoids use of the peer-link
 ```
 config t
 vpc domain ?
 peer-gateway
 ```
- **Layer3 router**: support routing over VPC, don't reduce the TTL.
 ```
 config t
 vpc domain ?
 peer-gateway
 layer 3 router
 ```

## Topology
???
## Basic Config (without features)

### Nexus_E
Basic Config (creating port-channel)
```
config t
feature lacp
feature interface-vlan

vlan 20
int vlan 20
ip add 20.20.20.30/24
no sh

int e1/3, e1/7
channel-group 21 mode active
! channel-group 21 force mode active
no sh
int po 21
sw
sw mode access
sw access vlan 20
no sh
```
Check port-channel
```
nexus_E(config-if)# sh port-chann sum
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
21    Po21(SU)    Eth      LACP      Eth1/3(P)    Eth1/7(P)    
nexus_E(config-if)# 
```

### Nexus_F
```
config t

feature vpc
feature lacp
feature interface-vlan

vlan 20
int vlan 20
ip add 20.20.20.10/24
no sh
exit

vpc domain 2
peer-keepalive destination 10.88.174.14 source 10.88.174.13 vrf management
role priority 100
exit

!peer-link
int e1/7
channel-group 20 mode active
no sh
exit
!
int po 20
sw
sw mode trunk
sw trunk allowed vlan 1,20
vpc peer-link
! bridge assurance enable by default or...
!spann port type network
no sh
```
Check vpc
```
nexus_F(config-if)# sh vpc
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 2   
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
Delay-restore status              : Timer is on.(timeout = 30s, 26s left)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po20   up     20                                                          
         
nexus_F(config-if)# 
```
Adding a device (Nexus_F)
```
 config t
    int e1/3
    channel-group 21 mode active
    exit
    int po 21
    sw
      sw mode access
      sw access vlan 20
      vpc 21
      no sh
```
Adding a device (vpc1 double-side)
```
 config t
 int e1/5,e1/8
 channel-group 1 mode active
 exit
 int po 1
 sw
 sw mode trunk
 sw trunk allowed vlan 1
 vpc 1
 no sh
```


### Nexus_G
Basic Config
```
config t

feature vpc
feature lacp
feature interface-vlan

vlan 20
int vlan 20
ip add 20.20.20.20/24
no sh
exit

vpc domain 2
peer-keepalive destination 10.88.174.13 source 10.88.174.14 vrf management
role priority 110
exit

!peer-link
int e1/4
channel-group 20 mode active
no sh
exit
!
int po 20
sw
sw mode trunk
sw trunk allowed vlan 1,20
vpc peer-link
! bridge assurance enable by default or...
!spann port type network
no sh
```
Check vpc
```
nexus_G(config-if)# sh vpc
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 2   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success 
Per-vlan consistency status       : success                       
Type-2 consistency status         : failed  
Type-2 inconsistency reason       : QoSMgr Qos configuration incompatible
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
1     Po20   up     20                                                          
         
nexus_G(config-if)# 
```
Adding a device (Nexus_F)
```
 config t
    int e1/3
    channel-group 21 mode active
    exit
    int po 21
    sw
      sw mode access
      sw access vlan 20
      vpc 21
      no sh
```
Adding a device (vpc1 double-side)
```
 config t
 int e1/5,e1/7
 channel-group 1 mode active
 exit
 int po 1
 sw
 sw mode trunk
 sw trunk allowed vlan 1
 vpc 1
 no sh
```

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
sw trunk allowed vlan 1,10
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
Adding a device (vpc2 double-side)
```
  config t
 int e1/4,e1/7
 channel-group 1 mode active
 exit
 int po 1
 sw
 sw mode trunk
 sw trunk allowed vlan 1
 vpc 1
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
sw trunk allowed vlan 1,10
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
Adding a device (vpc2 double-side)
```
   config t
 int e1/4,e1/7
 channel-group 1 mode active
 exit
 int po 1
 sw
 sw mode trunk
 sw trunk allowed vlan 1
 vpc 1
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

## Routing Config

### Nexus_E
```
config t
    feature ospf
    router ospf 1
      router-id 20.20.20.30
    exit
    int vlan 20
      ip router ospf 1 area 2
      
show ip ospf nei
```
### Nexus_F
```
config t
    feature ospf
    router ospf 1
      router-id 20.20.20.10
    exit
    int vlan 20
      ip router ospf 1 area 2
       exit
    int vlan 1
      ip add 1.1.2.10/16
      ip route ospf 1 area 0
      no sh
 
 show ip ospf nei
 ```
### Nexus_G
```
config t
    feature ospf
    router ospf 1
      router-id 20.20.20.20
    exit
    int vlan 20
      ip router ospf 1 area 2
      exit
    int vlan 1
      ip add 1.1.2.20/16
      ip route ospf 1 area 0
      no sh
      
      
      
 show ip ospf nei
 ```
### Nexus_H
```
config t
    feature ospf
    router ospf 1
      router-id 10.10.10.10
    exit
    int vlan 10
      ip router ospf 1 area 1
      exit
    int vlan 1
      ip add 1.1.1.10/16
      ip route ospf 1 area 0
      no sh
      
      
 show ip ospf nei
 ```
### Nexus_I
```
config t
    feature ospf
    router ospf 1
      router-id 10.10.10.20
    exit
    int vlan 10
      ip router ospf 1 area 1
      exit
    int vlan 1
      ip add 1.1.1.20/16
      ip route ospf 1 area 0
      no sh
      
      
 show ip ospf nei
 ```
### Nexus_J
```
config t
    feature ospf
    router ospf 1
      router-id 10.10.10.30
    exit
    int vlan 10
      ip router ospf 1 area 1
      exit
      
  
 show ip ospf nei
 ```
## Troubleshooting 
 Is possible that we have trouble making the ospf neighbors or making the pings.
 - For ping problems add **peer-gateway**:
 ```
 config t
 vpc domain ?
 peer-gateway
 ```
 - For neighbor problems add **layer3 router**:
 ```
 config t
 vpc domain ?
 peer-gateway
 layer 3 router
 ```
 - CPU packet capture
 ```
 ethanalyzer local interface inband display-filter STP limit-captured-frames 20
 ```

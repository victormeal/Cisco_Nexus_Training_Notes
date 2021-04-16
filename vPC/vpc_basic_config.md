# vPC Basic Config

## Left device
```
config t
vlan 100-110
exit

feature vpc
feature lacp

vpc domain 8
! ip addresses of mgmt0
peer-keepalive destination 10.88.174.191 source 10.88.174.190 vrf management 
role priority 100

int po 10
switchport
sw mode trunk
sw trunk allowed vlan 100-110
vpc 8
no sh

int e1/5-6
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 10 mode active
!channel-group 10 force mode active
no sh

!vPC peer-link
int po 20
switchport
sw mode trunk
sw trunk allowed vlan 100-110
spann port type network
vpc peer-link
no sh

int e1/3-4
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 20 mode active
!channel-group 20 force mode active
no sh
```
---
## Right Device
```
config t
vlan 100-110
exit

feature vpc
feature lacp

vpc domain 8
! ip addresses of mgmt0
peer-keepalive destination 10.88.174.190 source 10.88.174.191 vrf management 
role priority 90

int po 10
switchport
sw mode trunk
sw trunk allowed vlan 100-110
vpc 8
no sh

int e1/5-6
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 10 mode active
!channel-group 10 force mode active
no sh

!vPC peer-link
int po 20
switchport
sw mode trunk
sw trunk allowed vlan 100-110
spann port type network
vpc peer-link
no sh

int e1/3-4
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 20 mode active
!channel-group 20 force mode active
no sh
```
---
## Third Device
```
config t

int po 10
switchport
sw mode trunk
sw trunk allowed vlan 100-110
no sh

int range ten1/0/1-2
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 10 mode active
!channel-group 10 force mode active
no sh

int range ten1/0/3-4
switchport
sw mode trunk
sw trunk allowed vlan 100-110
channel-group 10 mode active
!channel-group 10 force mode active
no sh

```

---
## Show Commands
```
show vpc
show vpc brief
show port-channel summary
```
```
show vpc role
show lacp nei
show lacp nei nei po 20
```
```
show run int po 10
show run int mgmt0
```
```
sh vpc consistency-parameters global
sh vpc consistency-parameters vpc 8
sh vpc consistency-parameters int po 20
```
```
sh vdc
```

```

feature private-vlan

N9K-1# show run vlan

!Command: show running-config vlan
!Running configuration last done at: Tue Mar  1 01:55:20 2022
!Time: Tue Mar  1 01:59:06 2022

version 9.3(2) Bios:version 05.45 
vlan 1-2,20
vlan 2
  private-vlan isolated
vlan 20
  private-vlan primary
  private-vlan association 2

N9K-1# show run int e1/98-100

!Command: show running-config interface Ethernet1/98-100
!Running configuration last done at: Tue Mar  1 01:55:20 2022
!Time: Tue Mar  1 01:59:23 2022

version 9.3(2) Bios:version 05.45 

interface Ethernet1/98
  switchport
  switchport mode private-vlan host
  switchport private-vlan host-association 20 2
  no shutdown

interface Ethernet1/99
  switchport
  switchport mode private-vlan host
  switchport private-vlan host-association 20 2
  no shutdown

interface Ethernet1/100
  switchport
  switchport mode private-vlan promiscuous
  switchport private-vlan mapping 20 2
  no shutdown

N9K-1# 

```

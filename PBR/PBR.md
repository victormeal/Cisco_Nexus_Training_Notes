# PBR

## Quick commands
```
feature pbr

ip access-list X 
permit ip x.x.x.x

int e1/1
ip policy route-map NAME

route-map NAME permit 10
match ip address 1
set ip default next-hop y.y.y.y
```
## config (sh run)
```
switchB# sh run

!Command: show running-config
!Running configuration last done at: Tue May 11 21:41:09 2021
!Time: Tue May 11 21:42:41 2021

version 9.3(7) Bios:version 01.03 
hostname switchB
vdc switchB id 1
  limit-resource vlan minimum 16 maximum 4094
  limit-resource vrf minimum 2 maximum 4096
  limit-resource port-channel minimum 0 maximum 511
  limit-resource u4route-mem minimum 248 maximum 248
  limit-resource u6route-mem minimum 96 maximum 96
  limit-resource m4route-mem minimum 58 maximum 58
  limit-resource m6route-mem minimum 8 maximum 8

feature ospf
feature eigrp
feature pbr
feature interface-vlan

no password strength-check
username admin password 5 $5$KFECJK$8zLpLyze91l1LWYuABtsGCQSDNL8SD2jHpqol.jqUfD 
 role network-admin
no ip domain-lookup
ip access-list 1
  10 permit ip 192.168.11.11/32 192.168.5.1/32 
copp profile strict
snmp-server user admin network-admin auth md5 0x04f0f45a1c8e70477a03d78d3b4bd78a
 priv 0x04f0f45a1c8e70477a03d78d3b4bd78a localizedkey
rmon event 1 log trap public description FATAL(1) owner PMON@FATAL
rmon event 2 log trap public description CRITICAL(2) owner PMON@CRITICAL
rmon event 3 log trap public description ERROR(3) owner PMON@ERROR
rmon event 4 log trap public description WARNING(4) owner PMON@WARNING
rmon event 5 log trap public description INFORMATION(5) owner PMON@INFO

vlan 1

route-map myPBR permit 10
  match ip address 1 
  set ip next-hop 192.168.4.1 172.16.23.3    
vrf context management
  ip route 0.0.0.0/0 10.88.171.1


interface Vlan1


interface Ethernet1/44
  ip address 172.16.12.2/24
  ip router eigrp 1
  ip router ospf 1 area 0.0.0.0
  ip policy route-map myPBR
  no shutdown

interface Ethernet1/45
  ip address 172.16.23.2/24
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/46
  ip address 172.16.24.2/24
  ip router eigrp 1
  no shutdown


interface mgmt0
  vrf member management
  ip address 10.88.171.106/24

interface loopback0
  ip address 192.168.2.1/32
  ip router eigrp 1
  ip router ospf 1 area 0.0.0.0
icam monitor scale

line console
line vty
boot nxos bootflash:/nxos.9.3.7.bin 
router eigrp 1
router ospf 1

no logging console


```

## routing table

```

switchB# sh ip route 
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

172.16.12.0/24, ubest/mbest: 1/0, attached
    *via 172.16.12.2, Eth1/44, [0/0], 03:48:02, direct
172.16.12.2/32, ubest/mbest: 1/0, attached
    *via 172.16.12.2, Eth1/44, [0/0], 03:48:02, local
172.16.23.0/24, ubest/mbest: 1/0, attached
    *via 172.16.23.2, Eth1/45, [0/0], 03:46:31, direct
172.16.23.2/32, ubest/mbest: 1/0, attached
    *via 172.16.23.2, Eth1/45, [0/0], 03:46:31, local
172.16.24.0/24, ubest/mbest: 1/0, attached
    *via 172.16.24.2, Eth1/46, [0/0], 03:44:16, direct
172.16.24.2/32, ubest/mbest: 1/0, attached
    *via 172.16.24.2, Eth1/46, [0/0], 03:44:16, local
172.16.35.0/24, ubest/mbest: 1/0
    *via 172.16.23.3, Eth1/45, [110/8], 00:17:13, ospf-1, intra
172.16.45.0/24, ubest/mbest: 1/0
    *via 172.16.24.4, Eth1/46, [90/768], 00:17:36, eigrp-1, internal
192.168.1.1/32, ubest/mbest: 1/0
    *via 172.16.12.1, Eth1/44, [90/128576], 03:02:33, eigrp-1, internal
192.168.2.1/32, ubest/mbest: 2/0, attached
    *via 192.168.2.1, Lo0, [0/0], 03:04:12, local
    *via 192.168.2.1, Lo0, [0/0], 03:04:12, direct
192.168.3.1/32, ubest/mbest: 1/0
    *via 172.16.23.3, Eth1/45, [110/5], 00:26:14, ospf-1, intra
192.168.4.1/32, ubest/mbest: 1/0
    *via 172.16.24.4, Eth1/46, [90/128576], 00:25:19, eigrp-1, internal
192.168.5.1/32, ubest/mbest: 1/0
    *via 172.16.24.4, Eth1/46, [90/128832], 00:17:33, eigrp-1, internal
192.168.11.11/32, ubest/mbest: 1/0
    *via 172.16.12.1, Eth1/44, [90/128576], 00:09:49, eigrp-1, internal

switchB# 
```
## Tracert from other device
```
switchA# sh ip int bri

IP Interface Status for VRF "default"(1)
Interface            IP Address      Interface Status
Lo0                  192.168.1.1     protocol-up/link-up/admin-up       
Lo1                  192.168.11.11   protocol-up/link-up/admin-up       
Eth1/44              172.16.12.1     protocol-up/link-up/admin-up       
switchA# tracer 192.168.5.1 source-interface lo0
traceroute to 192.168.5.1 (192.168.5.1) from 192.168.1.1 (192.168.1.1), 30 hops max, 40 byte packets
 1  172.16.12.2 (172.16.12.2)  1.092 ms  0.649 ms  0.461 ms
 2  172.16.24.4 (172.16.24.4)  0.771 ms  0.617 ms  0.469 ms
 3  192.168.5.1 (192.168.5.1)  1.084 ms  0.907 ms  0.568 ms
switchA# tracer 192.168.5.1 source-interface lo1
traceroute to 192.168.5.1 (192.168.5.1) from 192.168.11.11 (192.168.11.11), 30 hops max, 40 byte packets
 1  172.16.12.2 (172.16.12.2)  1.23 ms  0.928 ms  0.707 ms
 2  172.16.23.3 (172.16.23.3)  0.723 ms  0.637 ms  0.523 ms
 3  192.168.5.1 (192.168.5.1)  0.948 ms  0.772 ms  0.572 ms
switchA# 
```

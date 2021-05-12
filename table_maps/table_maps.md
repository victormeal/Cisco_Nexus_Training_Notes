# Table-Map
- change the AD locally
## Quick config
```
feature ospf
feature eigrp

ip prefix-list 2 seq 5 permit 192.168.1.1/32 
route-map myTableMap permit 10
  match ip address prefix-list 2 
  set distance 89 

router eigrp 1
router ospf 1
  table-map myTableMap 
```

## Config
```
switchE# sh run

!Command: show running-config
!Running configuration last done at: Wed May 12 00:22:10 2021
!Time: Wed May 12 00:22:42 2021

version 10.1(1) Bios:version 01.02 
hostname switchE
vdc switchE id 1
  limit-resource vlan minimum 16 maximum 4094
  limit-resource vrf minimum 2 maximum 4096
  limit-resource port-channel minimum 0 maximum 511
  limit-resource u4route-mem minimum 248 maximum 248
  limit-resource u6route-mem minimum 96 maximum 96
  limit-resource m4route-mem minimum 58 maximum 58
  limit-resource m6route-mem minimum 8 maximum 8

feature ospf
feature eigrp
feature interface-vlan

no password strength-check
username admin password 5 $5$NCOFKM$./NUiS5E1QvnEpw80vSGZ0Bs6N2dSB4MEqSxwWQCg98 
 role network-admin
no ip domain-lookup
copp profile strict
snmp-server user admin network-admin auth md5 0xcb4f23e4e19deacaff8cceb8a89e3799
 priv aes-128 0xcb4f23e4e19deacaff8cceb8a89e3799 localizedkey
rmon event 1 log trap public description FATAL(1) owner PMON@FATAL
rmon event 2 log trap public description CRITICAL(2) owner PMON@CRITICAL
rmon event 3 log trap public description ERROR(3) owner PMON@ERROR
rmon event 4 log trap public description WARNING(4) owner PMON@WARNING
rmon event 5 log trap public description INFORMATION(5) owner PMON@INFO

vlan 1

ip prefix-list 2 seq 5 permit 192.168.1.1/32 
route-map myTableMap permit 10
  match ip address prefix-list 2 
  set distance 89 
vrf context management
  ip route 0.0.0.0/0 10.88.171.1

interface Vlan1

interface Ethernet1/46
  ip address 172.16.35.5/24
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/47
  ip address 172.16.45.5/24
  ip router eigrp 1
  no shutdown


interface mgmt0
  vrf member management
  ip address 10.88.171.109/24

interface loopback0
  ip address 192.168.5.1/32
  ip router eigrp 1
  ip router ospf 1 area 0.0.0.0
icam monitor scale

line console
line vty
boot nxos bootflash:/nxos.10.1.1.bin 
router eigrp 1
router ospf 1
  table-map myTableMap

no logging console


switchE#  
```

## Route Table before
```
switchE(config-router)# sh ip route
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

172.16.12.0/24, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/1024], 02:54:59, eigrp-1, internal
172.16.23.0/24, ubest/mbest: 1/0
    *via 172.16.35.3, Eth1/46, [110/8], 00:05:36, ospf-1, intra
172.16.24.0/24, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/768], 02:54:59, eigrp-1, internal
172.16.35.0/24, ubest/mbest: 1/0, attached
    *via 172.16.35.5, Eth1/46, [0/0], 06:20:52, direct
172.16.35.5/32, ubest/mbest: 1/0, attached
    *via 172.16.35.5, Eth1/46, [0/0], 06:20:52, local
172.16.45.0/24, ubest/mbest: 1/0, attached
    *via 172.16.45.5, Eth1/47, [0/0], 06:21:13, direct
172.16.45.5/32, ubest/mbest: 1/0, attached
    *via 172.16.45.5, Eth1/47, [0/0], 06:21:13, local
192.168.1.1/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/129088], 02:54:59, eigrp-1, internal
192.168.2.1/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/128832], 02:54:59, eigrp-1, internal
192.168.3.1/32, ubest/mbest: 1/0
    *via 172.16.35.3, Eth1/46, [110/5], 00:05:36, ospf-1, intra
192.168.4.1/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/128576], 02:54:59, eigrp-1, internal
192.168.5.1/32, ubest/mbest: 2/0, attached
    *via 192.168.5.1, Lo0, [0/0], 05:35:56, local
    *via 192.168.5.1, Lo0, [0/0], 05:35:56, direct
192.168.11.11/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/129088], 02:47:14, eigrp-1, internal

switchE(config-router)# 
```

## Route Table after
```
switchE(config-router)# sh ip route 
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

172.16.12.0/24, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/1024], 02:56:10, eigrp-1, internal
172.16.23.0/24, ubest/mbest: 1/0
    *via 172.16.35.3, Eth1/46, [110/8], 00:06:47, ospf-1, intra
172.16.24.0/24, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/768], 02:56:10, eigrp-1, internal
172.16.35.0/24, ubest/mbest: 1/0, attached
    *via 172.16.35.5, Eth1/46, [0/0], 06:22:03, direct
172.16.35.5/32, ubest/mbest: 1/0, attached
    *via 172.16.35.5, Eth1/46, [0/0], 06:22:03, local
172.16.45.0/24, ubest/mbest: 1/0, attached
    *via 172.16.45.5, Eth1/47, [0/0], 06:22:24, direct
172.16.45.5/32, ubest/mbest: 1/0, attached
    *via 172.16.45.5, Eth1/47, [0/0], 06:22:24, local
192.168.1.1/32, ubest/mbest: 1/0
    *via 172.16.35.3, Eth1/46, [89/13], 00:00:09, ospf-1, intra
192.168.2.1/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/128832], 02:56:10, eigrp-1, internal
192.168.3.1/32, ubest/mbest: 1/0
    *via 172.16.35.3, Eth1/46, [110/5], 00:06:47, ospf-1, intra
192.168.4.1/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/128576], 02:56:10, eigrp-1, internal
192.168.5.1/32, ubest/mbest: 2/0, attached
    *via 192.168.5.1, Lo0, [0/0], 05:37:07, local
    *via 192.168.5.1, Lo0, [0/0], 05:37:07, direct
192.168.11.11/32, ubest/mbest: 1/0
    *via 172.16.45.4, Eth1/47, [90/129088], 02:48:25, eigrp-1, internal

switchE(config-router)# 
```



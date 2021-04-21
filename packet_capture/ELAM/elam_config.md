# ELAM
https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/213848-nexus-9000-cloud-scale-asic-tahoe-nx-o.html

One packet, first match on interface

## config
1. Check that Nexus is compatible to run ELAM. Check in https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/213848-nexus-9000-cloud-scale-asic-tahoe-nx-o.html

```
nexus_01# show module
Mod Ports             Module-Type                      Model           Status
--- ----- ------------------------------------- --------------------- ---------
1    60   48x10/25G + 12x40/100G Ethernet Modul N9K-C93240YC-FX2      active *  

Mod  Sw                       Hw    Slot
---  ----------------------- ------ ----
1    9.3(7)                   2.0    NA  


Mod  MAC-Address(es)                         Serial-Num
---  --------------------------------------  ----------
1    b0-c5-3c-7b-e6-90 to b0-c5-3c-7b-e6-d7  FDO24481AQ5

Mod  Online Diag Status
---  ------------------
1    Pass

* this terminal session 
```
2. Verify port's ASIC, Slice and SrcId (you need to know in which interface to expect the packet)
```
nexus_01# sh hardware internal tah interface e1/2
#########################################
IfIndex: 0x1a000200
DstIndex: 6140
IfType: 26
Asic: 0
Asic: 0
AsicPort: 112
SrcId: 80
Slice: 1
PortOnSlice: 40
Table entries for interface Ethernet1/2

PifTable:
ENTRY: 40
        src_if_num : 1539
        niv_idx : 1539
        trunk_port : 1
        flow_collect_en : 1
        default_vlan : 1
        learn_enable : 1
        src_chip : 1
        fabric_port : 1
        outer_vlan_xlate_idx : 255
```
3. Attach to the module (int ex/y -> x is the module)
```
nexus_01# attach mod 1
```
4. Enter ELAM configuration mode and specify the proper ASIC from Step 2
```
module-1# debug platform internal tah elam asic 0
```
5. Configure the trigger - (There are many options you can specify here)
- asic #
- slice #
- in-select # -> 6 for outer headers, 7 for inner headers (GRE,VXLAN)
- use-src-id # (SrcId) see on step 2
```
module-1(TAH-elam)# trigger init asic 0 slice 1 lu-a2d 1 in-select ?
  10  {outer l4, inner l4, ieth}
  19  {udf_vec}
  6   {outer l2, outer l3, outer l4}
  7   {inner l2, inner l3, inner l4}
  8   {outer l2, inner l2, ieth}
  9   {outer l3, inner l3}

module-1(TAH-elam)# trigger init asic 0 slice 1 lu-a2d 1 in-select 6 out-select 0 use-src-id 80
Slot 1: param values: start asic 0, start slice 1, lu-a2d 1, in-select 6, out-se
lect 0, src_id 80
```
6. reset if there was previous config
```
module-1(TAH-elam-insel6)# reset
```
7. Set the ELAM triggers using SRC & DEST IP from this example
```
module-1(TAH-elam-insel6)# set outer ipv4 dst_ip 2.2.2.3 src_ip 1.1.1.6
```
8. start capture
```
module-1(TAH-elam-insel6)# start
```
9. see report
```
module-1(TAH-elam-insel6)# report
HEAVENLY ELAM REPORT SUMMARY
slot - 1, asic - 0, slice - 1
============================

Incoming Interface: Eth1/2
Src Idx : 0x603, Src BD : 3
Outgoing Interface Info: dmod 1, dpid 108
Dst Idx : 0x602, Dst BD : 2

Packet Type: IPv4

Dst MAC address: B0:C5:3C:7B:E6:97
Src MAC address: 44:AE:25:4B:F3:23
.1q Tag0 VLAN: 3,  cos = 0x0

Dst IPv4 address: 2.2.2.3
Src IPv4 address: 1.1.1.6
Ver     =  4, DSCP    =    0, Don't Fragment = 0
Proto   =  1, TTL     =  254, More Fragments = 0
Hdr len = 20, Pkt len =   84, Checksum       = 0x1ac1

L4 Protocol  : 1
ICMP type    : 8
ICMP code    : 0

Drop Info:
----------

LUA:
LUB:
LUC:
LUD:
Final Drops:

vntag:
vntag_valid    : 0
vntag_vir      : 0
vntag_svif     : 0

module-1(TAH-elam-insel6)# end
module-1# exit
nexus_01# 
```
10. check icoming and outgoing interfaces with the indexes
```
nexus_01# sh hardware internal tah niv_idx 0x603
niv_idx  : 1539
Interface: Ethernet1/2
nexus_01# sh hardware internal tah niv_idx 0x602
niv_idx  : 1538
Interface: Ethernet1/1
nexus_01# 
```

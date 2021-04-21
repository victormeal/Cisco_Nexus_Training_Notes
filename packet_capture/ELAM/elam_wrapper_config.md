# ELAM Wrapper
https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/213848-nexus-9000-cloud-scale-asic-tahoe-nx-o.html

One packet, first match on all interfaces

## config

```

nexus_01# sh module
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
nexus_01# debug platform internal tah elam
nexus_01(TAH-elam)# trigger init 
Slot 1: param values: start asic 0, start slice 0, lu-a2d 1, in-select 6, out-se
lect 0
nexus_01(TAH-elam-insel6)# reset
nexus_01(TAH-elam-insel6)# set outer ipv4 dest_ip 2.2.2.3 src_ip 1.1.1.6
                                           ^
% Invalid command at '^' marker.
nexus_01(TAH-elam-insel6)# set outer ipv4 dst_ip 2.2.2.3 src_ip 1.1.1.6
nexus_01(TAH-elam-insel6)# start
nexus_01(TAH-elam-insel6)# report
ELAM not triggered yet on slot - 1, asic - 0, slice - 0
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
Hdr len = 20, Pkt len =   84, Checksum       = 0xb953

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

nexus_01(TAH-elam-insel6)# 
```

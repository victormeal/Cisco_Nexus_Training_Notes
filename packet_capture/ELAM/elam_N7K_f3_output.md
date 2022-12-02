# elam_N7K_f3_output

https://techzone.cisco.com/t5/Troubleshooting-and-Tools/Troubleshoot-Nexus-Cheat-Sheet-for-Beginners/ta-p/1413193
https://techzone.cisco.com/t5/Hardware/Nexus-7000-M3-Module-ELAM-Procedure/ta-p/960214
https://techzone.cisco.com/t5/Hardware/Troubleshoot-Hardware-Forwarding-Issues-on-Nexus-7000-Series/ta-p/772937
https://techzone.cisco.com/t5/Nexus-7000/F3-Multidestination-OTV-Elam/ta-p/1006758

```
7010a# attach mod 1
Attaching to module 1 ...
To exit type 'exit', to abort type '$.'
module-1# elam asic flanker instance 0
module-1(fln-elam)# layer2
module-1(fln-l2-elam)# trigger dbus ipv4 if source-ipv4-address 10.200.1.80 destination-ipv4-address 239.0.1.2
module-1(fln-l2-elam)# trigger rbus ingress if trig
module-1(fln-l2-elam)# status
ELAM Slot 1 instance 0: L2 DBUS Configuration: trigger dbus ipv4 if source-ipv4-address 10.200.1.80 destination-ipv4-address 239.0.1.2
L2 DBUS: Configured
ELAM Slot 1 instance 0: L2 RBUS Configuration: trigger rbus ingress if trig
L2 RBUS: Configured
module-1(fln-l2-elam)# start
module-1(fln-l2-elam)# status
ELAM Slot 1 instance 0: L2 DBUS Configuration: trigger dbus ipv4 if source-ipv4-address 10.200.1.80 destination-ipv4-address 239.0.1.2
L2 DBUS: Armed
ELAM Slot 1 instance 0: L2 RBUS Configuration: trigger rbus ingress if trig
L2 RBUS: Armed
module-1(fln-l2-elam)# status
ELAM Slot 1 instance 0: L2 DBUS Configuration: trigger dbus ipv4 if source-ipv4-address 10.200.1.80 destination-ipv4-address 239.0.1.2
L2 DBUS: Armed
ELAM Slot 1 instance 0: L2 RBUS Configuration: trigger rbus ingress if trig
L2 RBUS: Armed
module-1(fln-l2-elam)# status
ELAM Slot 1 instance 0: L2 DBUS Configuration: trigger dbus ipv4 if source-ipv4-address 10.200.1.80 destination-ipv4-address 239.0.1.2
L2 DBUS: Triggered
ELAM Slot 1 instance 0: L2 RBUS Configuration: trigger rbus ingress if trig
L2 RBUS: Triggered
module-1(fln-l2-elam)# show dbus
cp = 0x205ad9f4, buf = 0x205ad9f4, end = 0x205b9d44
--------------------------------------------------------------------
Flanker Instance 00 - Capture Buffer On L2 DBUS:

Status(0x1102), TriggerWord(0x000), SampleStored(0x008),CaptureBufferPointer(
0x000)

is_l2_egress: 0x0000, data_size: 0x023
[000]: 605ca000 08610003 18900000 1b419000 00080420 00000000 01800100 0000000
0 00000000 00000000 00004017 80004080 1415afa7 10000000 03280040 00000000 003
fe739 3e100800 00000000 00000000 00000005 88405000 00000000 00000000 00000000
 00000000 00000000 00000000 00000000 00000000 00056400 a8778000 815c08c1 000c
8450 34503400
Printing packet 0
--------------------------------------------------------------------
                       L2 DBUS PRS MLH IPV4
--------------------------------------------------------------------
label-count         : 0x0             mc                  : 0x0
null-label-valid    : 0x0             null-label-exp      : 0x0
null-label-ttl      : 0x0             lbl0-vld            : 0x0
lbl0-eos            : 0x0             lbl0-lbl            : 0x0
lbl0-exp            : 0x0             lbl0-ttl            : 0x0
lbl1-exp            : 0x0             lbl1-ttl            : 0x0
ipv4                : 0x0             ipv6                : 0x0
l4-protocol         : 0x11            df                  : 0x1
mf                  : 0x0             frag                : 0x0
ttl                 : 0x10            l3-packet-length    : 0xc8
option              : 0x0             tos                 : 0xb8
sup-eid             : 0x0             header-type         : 0x1
error               : 0x0             redirect            : 0x0
port-id             : 0x3             last-ethertype      : 0x800
l2-frame-type       : 0x0             da-type             : 0x3
packet-type         : 0x0             l2-length-check     : 0x0
ip-da-multicast     : 0x1             ip-multicast        : 0x1      <<<<<<    
ip-multicast-control: 0x0             ids-check-fail      : 0x0
tr                  : 0x0             outer-cos           : 0x4
inner-cos           : 0x4             vqi-valid           : 0x0
vqi                 : 0x0             packet-length       : 0xda
vlan                : 0xc8            destination-index   : 0x0      <<<<<<  e1/1, po 2
source-index        : 0x402           bundle-port         : 0x1
acos                : 0x0             outer-drop-eligibility: 0x0
inner-drop-eligibility: 0x0             sg-tag              : 0x0
rbh                 : 0x0             vsl-num             : 0x0
inband-flow-creation-deletion: 0x0             ignore-qoso         : 0x0

ignore-qosi         : 0x0             ignore-aclo         : 0x0
ignore-acli         : 0x0             index-direct        : 0x0
no-stats            : 0x0             dont-forward        : 0x0
notify-index-learn  : 0x1             notify-new-learn    : 0x1
disable-new-learn   : 0x0             disable-index-learn : 0x0
dont-learn          : 0x0             bpdu                : 0x0
ff                  : 0x0             rf                  : 0x0
ccc                 : 0x0             l2                  : 0x0
rdt                 : 0x0             dft                 : 0x0
dfst                : 0x0             status-ce-1q        : 0x0
status-is-1q        : 0x1             trill-encap         : 0x0
mim-valid           : 0x0             dtag-ttl            : 0x0
dtag-ftag           : 0x0             valid               : 0x1
erspan-kpa-valid    : 0x0             recir-shim-vxlan-src-peer-id: 0x0

vn-valid            : 0x0             source-vif          : 0x0
destination-vif     : 0x0             vn-p                : 0x0
sequence-number     : 0xca            vl                  : 0x0
inner-de-valid      : 0x0             de-cfi              : 0x0
second-inner-cos    : 0x0             tunnel-type         : 0x2
--------------------------------------------------------------------
                       UDP OTV/LISP TUNNEL BNDL
--------------------------------------------------------------------
vlan-tag-valid: 0x0             segment-id-valid: 0x0
vl: 0x0             de: 0x0
sgt-valid: 0x0             inner-ip-ttl: 0xff
ip-da-multicast: 0x1             lisp-inst-id: 0x39c9f0
lisp-flags: 0x80            isis-mac-da-valid: 0x0
type: 0x2
shim-valid          : 0x0
segment-id-valid    : 0x0             copp                : 0x0
dti-type-vpnid      : 0x0             segment-id          : 0x0
ib-length-bundle    : 0x58840         mlh-type            : 0x5
ulh-type            : 0x4
source-ipv4-address: 10.200.1.80                                        <<<<<<  multicast source
destination-ipv4-address: 239.0.1.2                                       <<<<<<  multicast group
mim-destination-mac-address : 0000.0000.0000
mim-source-mac-address : 0000.0000.0000
destination-mac-address : 0100.5e00.0102
source-mac-address : 0050.56be.9c40



module-1(fln-l2-elam)# show rbu
cp = 0x205d23f0, buf = 0x205d23f0, end = 0x205de740
--------------------------------------------------------------------
Flanker Instance 00 - Capture Buffer On L2 RBUS:

Status(0x1102), TriggerWord(0x000), SampleStored(0x008),CaptureBufferPointer(
0x000)

is_l2_egress: 0x0000, data_size: 0x018
[000]: 00613330 0000001b 40000000 06500000 00000000 00000000 0000001f 65e805c
0 20217170 00000000 0c01125f f7846741 00832100 00020000 00000000 00000000 000
00000 00000000 00104100 00140000 00100010 05e00008 11c01700 00000028
Printing packet 0
--------------------------------------------------------------------
                       L2 RBUS INGRESS CONTENT
--------------------------------------------------------------------
pad                 : 0x184cc         valid               : 0x1
l2-rbus-trigger     : 0x1             sequence-number     : 0xca
rit-ipv4-id         : 0x0             ipv4-tunnel-encap   : 0x0
rit-mpls-rw         : 0x0             ml2-ptr             : 0x0
ml3-ptr             : 0x0             mark                : 0x0
result-cap3         : 0x0             di1-v5-delta-length : 0x0
di1-v5-delta-length-plus: 0x0             di1-v4-delta-length : 0x0

di1-v4-delta-length-plus: 0x0             di2-delta-length    : 0x0

di2-delta-length-plus: 0x0             ml2-delta-length    : 0x0
ml2-delta-length-plus: 0x0             ml3-delta-length    : 0x0
ml3-delta-length-plus: 0x0             s-vector            : 0x0
lcpu-ff-valid       : 0x0             sup-di-vqi          : 0x0
erspan-term-index-dir: 0x0             erspan-buffer-check : 0x0
l2-tunnel-decapped  : 0x0             l3-delta-length     : 0x0
rit-crc16-valid     : 0x1             rit-crc16           : 0xf65e
vntag-p             : 0x1             frr-recirc          : 0x0
ingress-lif         : 0x2e            earl-proxy-vld      : 0x0
md-di-vld           : 0x0             rc                  : 0x0
segment-id-valid    : 0x0             ttl-out             : 0x10
ttl-mid             : 0x10            tos-out             : 0xb8
tos-in              : 0xb8            orig-vlan1          : 0x0
vlan1               : 0x0             source-peer-id      : 0x0
final-ignore-qoso   : 0x0             port-id             : 0x3
cr-type             : 0x0             pup-packet          : 0x0
bpdu                : 0x0             vdc                 : 0x0
tr                  : 0x0             de                  : 0x0
cos                 : 0x1             inner-drop-eligibility: 0x0
inner-cos           : 0x1             acos                : 0x9
di-ltl-index        : 0x7fde          l3-multicast-di     : 0x119d        <<<<<<
source-index        : 0x402           vlan                : 0xc8
index-direct        : 0x0             di1-valid           : 0x1
vqi                 : 0x0             di2-valid           : 0x0
v5-fpoe-idx         : 0x4             di2-fpoe-idx        : 0x0
l3-multicast-v5     : 0x0             dft                 : 0x0
dfst                : 0x0             l3-learning-ff      : 0x0
result-rbh          : 0x0             di2-cr-type         : 0x0
result-2            : 0x0             dtag-ftag           : 0x0
dtag-ttl            : 0x0             mac-in-mac-op       : 0x0
dvif                : 0x0             result-cap1         : 0x1
result-cap2         : 0x0             erspan-term         : 0x0
erspan-decap        : 0x0             dont-learn          : 0x0
routed-frame        : 0x0             copy-cause          : 0x104
l2-copy-cause       : 0x0             l3-rit-ptr          : 0x14
sg-tag              : 0x0             trill-nh-id         : 0x0
ttl-in              : 0x10            fc-up               : 0x0
up-did              : 0x0             did                 : 0x1005e
up-sid              : 0x0             sid                 : 0x102
shim-l2-tunnel-encap: 0x0             shim-ls-hash        : 0xe
shim-rc             : 0x0             shim-lif            : 0x2e
shim-replication-pkt: 0x0             shim-router-mac     : 0x0
shim-mark-enable    : 0x0             shim-qos-group-id   : 0x0
shim-destination-table-index: 0x14            shim-acos-preserve  : 0x0

mim-destination-mac-address : 0000.0000.0000
mim-source-mac-address : 0000.0000.0000



module-1(fln-l2-elam)#


7010a# show system internal pixm info ltl 0x402

PC_TYPE    PORT    LTL      RES_ID       LTL_FLAG      CB_FLAG    PC_NODE_FLA
G   MEMB_CNT
-----------------------------------------------------------------------------
-
Normal    Po3   0x0402    0x16000002   0x00000000   0x00000002   0x00000000
 2
RBH Mode: NON-MODULO
Member rbh rbh_cnt
Eth1/3   0x000000f0   0x04
Eth1/4   0x0000000f   0x04

CBL Check States: Ingress: Enabled; Egress: Enabled

VLAN| BD| BD-St              | CBL St & Direction:
--------------------------------------------------
1 | 0x1 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
2 | 0x19 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
3 | 0x1a | INCLUDE_IF_IN_BD  | FORWARDING (Both)
4 | 0x1b | INCLUDE_IF_IN_BD  | FORWARDING (Both)
5 | 0x1c | INCLUDE_IF_IN_BD  | FORWARDING (Both)
6 | 0x1d | INCLUDE_IF_IN_BD  | FORWARDING (Both)
7 | 0x1e | INCLUDE_IF_IN_BD  | FORWARDING (Both)
8 | 0x1f | INCLUDE_IF_IN_BD  | FORWARDING (Both)
9 | 0x20 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
10 | 0x21 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
12 | 0x22 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
14 | 0x23 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
15 | 0x24 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
16 | 0x25 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
17 | 0x26 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
18 | 0x27 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
19 | 0x28 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
20 | 0x29 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
21 | 0x2a | INCLUDE_IF_IN_BD  | FORWARDING (Both)
22 | 0x2b | INCLUDE_IF_IN_BD  | FORWARDING (Both)
24 | 0x2c | INCLUDE_IF_IN_BD  | FORWARDING (Both)
25 | 0x2d | INCLUDE_IF_IN_BD  | FORWARDING (Both)
26 | 0x2e | INCLUDE_IF_IN_BD  | FORWARDING (Both)
28 | 0x2f | INCLUDE_IF_IN_BD  | FORWARDING (Both)
30 | 0x30 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
35 | 0x31 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
40 | 0x32 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
45 | 0x33 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
50 | 0x34 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
55 | 0x35 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
56 | 0x36 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
60 | 0x37 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
72 | 0x5c | INCLUDE_IF_IN_BD  | FORWARDING (Both)
80 | 0x38 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
81 | 0x39 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
98 | 0x3a | INCLUDE_IF_IN_BD  | FORWARDING (Both)
100 | 0x3b | INCLUDE_IF_IN_BD  | FORWARDING (Both)
101 | 0x3c | INCLUDE_IF_IN_BD  | FORWARDING (Both)
105 | 0x3d | INCLUDE_IF_IN_BD  | FORWARDING (Both)
109 | 0x3e | INCLUDE_IF_IN_BD  | FORWARDING (Both)
110 | 0x3f | INCLUDE_IF_IN_BD  | FORWARDING (Both)
117 | 0x40 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
120 | 0x41 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
126 | 0x42 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
128 | 0x43 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
130 | 0x44 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
132 | 0x45 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
134 | 0x46 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
139 | 0x47 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
140 | 0x48 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
142 | 0x5b | INCLUDE_IF_IN_BD  | FORWARDING (Both)
145 | 0x49 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
150 | 0x4a | INCLUDE_IF_IN_BD  | FORWARDING (Both)
155 | 0x4b | INCLUDE_IF_IN_BD  | FORWARDING (Both)
160 | 0x4c | INCLUDE_IF_IN_BD  | FORWARDING (Both)
170 | 0x4d | INCLUDE_IF_IN_BD  | FORWARDING (Both)
179 | 0x4e | INCLUDE_IF_IN_BD  | FORWARDING (Both)
180 | 0x4f | INCLUDE_IF_IN_BD  | FORWARDING (Both)
181 | 0x50 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
182 | 0x51 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
190 | 0x52 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
191 | 0x53 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
192 | 0x54 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
200 | 0x55 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
201 | 0x56 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
202 | 0x57 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
850 | 0x58 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
999 | 0x59 | INCLUDE_IF_IN_BD  | FORWARDING (Both)
1010 | 0x5a | INCLUDE_IF_IN_BD  | FORWARDING (Both)
686 | 0x5d | INCLUDE_IF_IN_BD  | FORWARDING (Both)


Member info
------------------
Type            LTL
---------------------------------
PORT_CHANNEL   Po3
MCAST_GROUP     0x7f92
MCAST_GROUP     0x7f82
MCAST_GROUP     0x7f9c
MCAST_GROUP     0x7f90
MCAST_GROUP     0x7f96
MCAST_GROUP     0x7f7a
MCAST_GROUP     0x7f80
MCAST_GROUP     0x7fec
MCAST_GROUP     0x7fd8
MCAST_GROUP     0x7fdc
FLOOD_W_FPOE   0x8020
FLOOD_W_FPOE   0x801b
MCAST_GROUP     0x7fe0
FLOOD_W_FPOE   0x801d
FLOOD_W_FPOE   0x801c
FLOOD_W_FPOE   0x8001
FLOOD_W_FPOE   0x801f
FLOOD_W_FPOE   0x8022
FLOOD_W_FPOE   0x8025
FLOOD_W_FPOE   0x8024
FLOOD_W_FPOE   0x8023
FLOOD_W_FPOE   0x8021
FLOOD_W_FPOE   0x8026
FLOOD_W_FPOE   0x802a
FLOOD_W_FPOE   0x8029
FLOOD_W_FPOE   0x8027
FLOOD_W_FPOE   0x802d
FLOOD_W_FPOE   0x802c
FLOOD_W_FPOE   0x8028
FLOOD_W_FPOE   0x802b
FLOOD_W_FPOE   0x802e
FLOOD_W_FPOE   0x802f
FLOOD_W_FPOE   0x8033
FLOOD_W_FPOE   0x8030
FLOOD_W_FPOE   0x8032
FLOOD_W_FPOE   0x8037
FLOOD_W_FPOE   0x8034
FLOOD_W_FPOE   0x8031
MCAST_GROUP     0x7f9a
FLOOD_W_FPOE   0x803a
FLOOD_W_FPOE   0x803b
FLOOD_W_FPOE   0x8040
FLOOD_W_FPOE   0x803f
FLOOD_W_FPOE   0x803e
FLOOD_W_FPOE   0x803c
MCAST_GROUP     0x7fae
FLOOD_W_FPOE   0x8048
FLOOD_W_FPOE   0x8042
FLOOD_W_FPOE   0x8044
FLOOD_W_FPOE   0x8041
FLOOD_W_FPOE   0x803d
FLOOD_W_FPOE   0x8043
MCAST_GROUP     0x7fa0
FLOOD_W_FPOE   0x8049
FLOOD_W_FPOE   0x804b
MCAST_GROUP     0x7f8a
FLOOD_W_FPOE   0x804d
FLOOD_W_FPOE   0x8054
FLOOD_W_FPOE   0x8051
FLOOD_W_FPOE   0x804f
MCAST_GROUP     0x7f94
FLOOD_W_FPOE   0x8052
MCAST_GROUP     0x7f9e
FLOOD_W_FPOE   0x804c
FLOOD_W_FPOE   0x804e
FLOOD_W_FPOE   0x8050
FLOOD_W_FPOE   0x8055
FLOOD_W_FPOE   0x8053
FLOOD_W_FPOE   0x8057
FLOOD_W_FPOE   0x8056
FLOOD_W_FPOE   0x8059
MCAST_GROUP     0x7fd6
FLOOD_W_FPOE   0x805a
FLOOD_W_FPOE   0x8019
FLOOD_W_FPOE   0x805b
FLOOD_W_FPOE   0x8045
FLOOD_W_FPOE   0x801e
FLOOD_W_FPOE   0x805d
FLOOD_W_FPOE   0x805c
FLOOD_W_FPOE   0x8038
FLOOD_W_FPOE   0x8036
FLOOD_W_FPOE   0x8039
FLOOD_W_FPOE   0x8035
FLOOD_W_FPOE   0x804a
FLOOD_W_FPOE   0x8046
MCAST_GROUP     0x7fc2
FLOOD_W_FPOE   0x8058
MCAST_GROUP     0x7f98
FLOOD_W_FPOE   0x8047
FLOOD_W_FPOE   0x801a


7010a# show system internal pixm info ltl 0x0

Member info
------------------
Type            LTL
---------------------------------
PHY_PORT       Eth1/1


7010a#

7010a# show run int po 3

!Command: show running-config interface port-channel3
!Time: Thu Dec  1 04:07:54 2022

version 8.1(2a)

interface port-channel3
  description LINK TO 5548B
  switchport
  switchport mode trunk

7010a# show run int e1/1

!Command: show running-config interface Ethernet1/1
!Time: Thu Dec  1 04:08:05 2022

version 8.1(2a)

interface Ethernet1/1
  description LINK TO 5548A
  switchport
  switchport mode trunk
  channel-group 2
  no shutdown


7010a# show run int po 2

!Command: show running-config interface port-channel2
!Time: Thu Dec  1 04:08:18 2022

version 8.1(2a)

interface port-channel2
  description LINK TO 5548A
  switchport
  switchport mode trunk


7010a# show cdp neighbors interface po2,po3
Capability Codes: R - Router, T - Trans-Bridge, B - Source-Route-Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater,
                  V - VoIP-Phone, D - Remotely-Managed-Device,
                  s - Supports-STP-Dispute

Device-ID          Local Intrfce  Hldtme Capability  Platform        Port ID
cod_5548a.dccd.cc.ca.us(SSI16010CF3)
                    Eth1/1         124    S I s     N5K-C5548UP      Eth1/29

cod_5548a.dccd.cc.ca.us(SSI16010CF3)
                    Eth1/2         124    S I s     N5K-C5548UP      Eth1/30

cod_5548b.dccd.cc.ca.us(SSI15510NNN)
                    Eth1/3         171    S I s     N5K-C5548UP      Eth1/29

cod_5548b.dccd.cc.ca.us(SSI15510NNN)
                    Eth1/4         171    S I s     N5K-C5548UP      Eth1/30


Total entries displayed: 4
7010a#






```

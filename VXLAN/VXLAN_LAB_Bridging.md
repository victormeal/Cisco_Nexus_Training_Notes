# VXLAN Bridging Config
- Bridging: Same subnet but divided by L3
## Spine
```
SPINE_01(config)# sh run

hostname SPINE_01

feature ospf
feature pim

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1

interface Ethernet1/1
  ip address 192.168.1.1/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/2
  ip address 192.168.2.1/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface loopback0
  ip address 7.7.7.7/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
 
router ospf 1

SPINE_01(config)# 
```
----
## LeafA
```
Leaf_A# sh run

hostname Leaf_A

feature ospf
feature pim
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 5000

interface Vlan1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 5000 mcast-group 239.1.1.1

interface Ethernet1/1
  ip address 192.168.1.2/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/46
  switchport
  switchport access vlan 10
  no shutdown


interface loopback0
  ip address 11.11.11.11/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

router ospf 1

Leaf_A#
```

----
## LeafB
```
Leaf_B# sh run

hostname Leaf_B

feature ospf
feature pim
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

ip pim rp-address 7.7.7.7 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10
vlan 10
  vn-segment 5000

interface Vlan1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 5000 mcast-group 239.1.1.1

interface Ethernet1/1
  ip address 192.168.2.2/30
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/45
  switchport
  switchport access vlan 10
  no shutdown

interface loopback0
  ip address 22.22.22.22/32
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

router ospf 1


Leaf_B# 
```
----
## MAC Address table
- Leaf A
```
Leaf_A# sh mac address-table
Legend: 
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*   10     44ae.254b.f2bf   dynamic  0         F      F    nve1(22.22.22.22)
*   10     e8eb.3467.9e1b   dynamic  0         F      F    Eth1/46
G    -     e8eb.347e.480f   static   -         F      F    sup-eth1(R)
Leaf_A# 
```
----
## Packet Capture (Ping HostA (1.1.1.10) ---> HostB (1.1.1.20))
- Capture on Leaf_A`s int e1/1
- Span to CPU
```
ethanalyzer local interface inband mirror display-filter icmp limit-captured-frames 0 detail 
```
```
Frame 7 (148 bytes on wire, 148 bytes captured)
    Arrival Time: May 21, 2021 21:24:48.390645000
    [Time delta from previous captured frame: 0.000554000 seconds]
    [Time delta from previous displayed frame: 0.000554000 seconds]
    [Time since reference or first frame: 0.002447000 seconds]
    Frame Number: 7
    Frame Length: 148 bytes
    Capture Length: 148 bytes
    [Frame is marked: False]
    [Protocols in frame: eth:ip:udp:vxlan:eth:ip:icmp:data]
Ethernet II, Src: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f), Dst: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
    Destination: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        Address: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Source: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f)
        Address: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Type: IP (0x0800)
Internet Protocol, Src: 11.11.11.11 (11.11.11.11), Dst: 22.22.22.22 (22.22.22.22)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 134
    Identification: 0x55aa (21930)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 255
    Protocol: UDP (0x11)
    Header checksum: 0x237b [correct]
        [Good: True]
        [Bad : False]
    Source: 11.11.11.11 (11.11.11.11)
    Destination: 22.22.22.22 (22.22.22.22)
User Datagram Protocol, Src Port: 22257 (22257), Dst Port: 4789 (4789)
    Source port: 22257 (22257)
    Destination port: 4789 (4789)
    Length: 114
    Checksum: 0x0000 (none)
        Good Checksum: False
        Bad Checksum: False
Virtual eXtensible Local Area Network
    Flags: 0x08
        0... .... = Reserved(R): False
        .0.. .... = Reserved(R): False
        ..0. .... = Reserved(R): False
        ...0 .... = Reserved(R): False
        .... 1... = VXLAN Network ID(VNI): True
        ...0 .... = Reserved(R): False
        ...0 .... = Reserved(R): False
        ...0 .... = Reserved(R): False
    Reserved: 0x000000
    VXLAN Network Identifier (VNI): 5000
    Reserved: 0
Ethernet II, Src: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b), Dst: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf)
    Destination: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf)
        Address: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Source: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b)
        Address: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Type: IP (0x0800)
Internet Protocol, Src: 1.1.1.10 (1.1.1.10), Dst: 1.1.1.20 (1.1.1.20)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 84
    Identification: 0xce25 (52773)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 255
    Protocol: ICMP (0x01)
    Header checksum: 0xe963 [correct]
        [Good: True]
        [Bad : False]
    Source: 1.1.1.10 (1.1.1.10)
    Destination: 1.1.1.20 (1.1.1.20)
Internet Control Message Protocol
    Type: 8 (Echo (ping) request)
    Code: 0 ()
    Checksum: 0x3f12 [correct]
    Identifier: 0x5267
    Sequence number: 768 (0x0300)
    Data (56 bytes)

0000  1d 25 a8 60 bd 79 0b 00 cd ab 00 00 cd ab 00 00   .%.`.y..........
0010  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0020  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0030  30 31 32 33 34 35 36 37                           01234567
        Data: 1D25A860BD790B00CDAB0000CDAB0000CDAB0000CDAB0000...
        [Length: 56]
```
```
Frame 8 (148 bytes on wire, 148 bytes captured)
    Arrival Time: May 21, 2021 21:24:48.390857000
    [Time delta from previous captured frame: 0.000212000 seconds]
    [Time delta from previous displayed frame: 0.000212000 seconds]
    [Time since reference or first frame: 0.002659000 seconds]
    Frame Number: 8
    Frame Length: 148 bytes
    Capture Length: 148 bytes
    [Frame is marked: False]
    [Protocols in frame: eth:ip:udp:vxlan:eth:ip:icmp:data]
Ethernet II, Src: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97), Dst: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f)
    Destination: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f)
        Address: e8:eb:34:7e:48:0f (e8:eb:34:7e:48:0f)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Source: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        Address: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Type: IP (0x0800)
Internet Protocol, Src: 22.22.22.22 (22.22.22.22), Dst: 11.11.11.11 (11.11.11.11)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 134
    Identification: 0x55aa (21930)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 254
    Protocol: UDP (0x11)
    Header checksum: 0x247b [correct]
        [Good: True]
        [Bad : False]
    Source: 22.22.22.22 (22.22.22.22)
    Destination: 11.11.11.11 (11.11.11.11)
User Datagram Protocol, Src Port: 14184 (14184), Dst Port: 4789 (4789)
    Source port: 14184 (14184)
    Destination port: 4789 (4789)
    Length: 114
    Checksum: 0x0000 (none)
        Good Checksum: False
        Bad Checksum: False
Virtual eXtensible Local Area Network
    Flags: 0x08
        0... .... = Reserved(R): False
        .0.. .... = Reserved(R): False
        ..0. .... = Reserved(R): False
        ...0 .... = Reserved(R): False
        .... 1... = VXLAN Network ID(VNI): True
        ...0 .... = Reserved(R): False
        ...0 .... = Reserved(R): False
        ...0 .... = Reserved(R): False
    Reserved: 0x000000
    VXLAN Network Identifier (VNI): 5000
    Reserved: 0
Ethernet II, Src: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf), Dst: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b)
    Destination: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b)
        Address: e8:eb:34:67:9e:1b (e8:eb:34:67:9e:1b)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Source: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf)
        Address: 44:ae:25:4b:f2:bf (44:ae:25:4b:f2:bf)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    Type: IP (0x0800)
Internet Protocol, Src: 1.1.1.20 (1.1.1.20), Dst: 1.1.1.10 (1.1.1.10)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 84
    Identification: 0xce25 (52773)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 255
    Protocol: ICMP (0x01)
    Header checksum: 0xe963 [correct]
        [Good: True]
        [Bad : False]
    Source: 1.1.1.20 (1.1.1.20)
    Destination: 1.1.1.10 (1.1.1.10)
Internet Control Message Protocol
    Type: 0 (Echo (ping) reply)
    Code: 0 ()
    Checksum: 0x4712 [correct]
    Identifier: 0x5267
    Sequence number: 768 (0x0300)
    Data (56 bytes)

0000  1d 25 a8 60 bd 79 0b 00 cd ab 00 00 cd ab 00 00   .%.`.y..........
0010  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0020  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0030  30 31 32 33 34 35 36 37                           01234567
        Data: 1D25A860BD790B00CDAB0000CDAB0000CDAB0000CDAB0000...
        [Length: 56]
```

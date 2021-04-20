# Ethanalyzer
only captures packets that go to CPU

```
nexus_01# ethanalyzer local interface inband display-filter icmp limit-captured-frames 0
Capturing on inband
2021-04-20 23:44:39.163923      1.1.1.6 -> 2.2.2.1      ICMP Echo (ping) request
2021-04-20 23:44:39.164399      2.2.2.1 -> 1.1.1.6      ICMP Echo (ping) reply
2021-04-20 23:44:39.165095      1.1.1.6 -> 2.2.2.1      ICMP Echo (ping) request
2021-04-20 23:44:39.165336      2.2.2.1 -> 1.1.1.6      ICMP Echo (ping) reply
2021-04-20 23:44:39.165750      1.1.1.6 -> 2.2.2.1      ICMP Echo (ping) request
2021-04-20 23:44:39.165961      2.2.2.1 -> 1.1.1.6      ICMP Echo (ping) reply
2021-04-20 23:44:39.166421      1.1.1.6 -> 2.2.2.1      ICMP Echo (ping) request
2021-04-20 23:44:39.166627      2.2.2.1 -> 1.1.1.6      ICMP Echo (ping) reply
2021-04-20 23:44:39.167233      1.1.1.6 -> 2.2.2.1      ICMP Echo (ping) request
```

```
nexus_01# ethanalyzer local interface inband display-filter icmp limit-captured-frames 0 detail
Capturing on inband
Frame 9 (102 bytes on wire, 102 bytes captured)
    Arrival Time: Apr 20, 2021 23:45:55.796935000
    [Time delta from previous captured frame: 0.095394000 seconds]
    [Time delta from previous displayed frame: 1.228385000 seconds]
    [Time since reference or first frame: 1.228385000 seconds]
    Frame Number: 9
    Frame Length: 102 bytes
    Capture Length: 102 bytes
    [Frame is marked: False]
    [Protocols in frame: eth:vlan:ip:icmp:data]
Ethernet II, Src: 44:ae:25:4b:f3:23 (44:ae:25:4b:f3:23), Dst: b0:c5:3c:7b:e6:97 
(b0:c5:3c:7b:e6:97)
    Destination: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        Address: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory
 default)
    Source: 44:ae:25:4b:f3:23 (44:ae:25:4b:f3:23)
        Address: 44:ae:25:4b:f3:23 (44:ae:25:4b:f3:23)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory
 default)
    Type: 802.1Q Virtual LAN (0x8100)
802.1Q Virtual LAN, PRI: 0, CFI: 0, ID: 3
    000. .... .... .... = Priority: 0
    ...0 .... .... .... = CFI: 0
    .... 0000 0000 0011 = ID: 3
    Type: IP (0x0800)
Internet Protocol, Src: 1.1.1.6 (1.1.1.6), Dst: 2.2.2.1 (2.2.2.1)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 84
    Identification: 0xe0d5 (57557)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 254
    Protocol: ICMP (0x01)
    Header checksum: 0xd5c9 [correct]
        [Good: True]
        [Bad : False]
    Source: 1.1.1.6 (1.1.1.6)
    Destination: 2.2.2.1 (2.2.2.1)
Internet Control Message Protocol
    Type: 8 (Echo (ping) request)
    Code: 0 ()
    Checksum: 0x0305 [correct]
    Identifier: 0x030f
    Sequence number: 16656 (0x4110)
    Data (56 bytes)

0000  cf 67 7f 60 88 8c 04 00 cd ab 00 00 cd ab 00 00   .g.`............
0010  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0020  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0030  30 31 32 33 34 35 36 37                           01234567
        Data: CF677F60888C0400CDAB0000CDAB0000CDAB0000CDAB0000...
        [Length: 56]

Frame 10 (102 bytes on wire, 102 bytes captured)
    Arrival Time: Apr 20, 2021 23:45:55.797171000
    [Time delta from previous captured frame: 0.000236000 seconds]
    [Time delta from previous displayed frame: 0.000236000 seconds]
    [Time since reference or first frame: 1.228621000 seconds]
    Frame Number: 10
    Frame Length: 102 bytes
    Capture Length: 102 bytes
    [Frame is marked: False]
    [Protocols in frame: eth:vlan:ip:icmp:data]
Ethernet II, Src: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97), Dst: 44:ae:25:4b:f3:23 
(44:ae:25:4b:f3:23)
    Destination: 44:ae:25:4b:f3:23 (44:ae:25:4b:f3:23)
        Address: 44:ae:25:4b:f3:23 (44:ae:25:4b:f3:23)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory
 default)
    Source: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        Address: b0:c5:3c:7b:e6:97 (b0:c5:3c:7b:e6:97)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory
 default)
    Type: 802.1Q Virtual LAN (0x8100)
802.1Q Virtual LAN, PRI: 0, CFI: 0, ID: 3
    000. .... .... .... = Priority: 0
    ...0 .... .... .... = CFI: 0
    .... 0000 0000 0011 = ID: 3
    Type: IP (0x0800)
Internet Protocol, Src: 2.2.2.1 (2.2.2.1), Dst: 1.1.1.6 (1.1.1.6)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 84
    Identification: 0xe0d5 (57557)
    Flags: 0x00
        0.. = Reserved bit: Not Set
        .0. = Don't fragment: Not Set
        ..0 = More fragments: Not Set
    Fragment offset: 0
    Time to live: 255
    Protocol: ICMP (0x01)
    Header checksum: 0xd4c9 [correct]
        [Good: True]
        [Bad : False]
    Source: 2.2.2.1 (2.2.2.1)
    Destination: 1.1.1.6 (1.1.1.6)
Internet Control Message Protocol
    Type: 0 (Echo (ping) reply)
    Code: 0 ()
    Checksum: 0x0b05 [correct]
    Identifier: 0x030f
    Sequence number: 16656 (0x4110)
    Data (56 bytes)

0000  cf 67 7f 60 88 8c 04 00 cd ab 00 00 cd ab 00 00   .g.`............
0010  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0020  cd ab 00 00 cd ab 00 00 cd ab 00 00 cd ab 00 00   ................
0030  30 31 32 33 34 35 36 37                           01234567
        Data: CF677F60888C0400CDAB0000CDAB0000CDAB0000CDAB0000...
        [Length: 56]
```
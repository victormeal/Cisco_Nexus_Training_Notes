# Scapy
```
conf ; feature bash ; run bash

sudo su -
cd /
cd bootflash/scapy-2.4.0
python
```

```python
from scapy.all import *

l2=Ether()
l2.src='00:ea:bd:85:cd:c9'
l2.dst='70:6d:15:41:67:9b'
l3=IP()
l3.src='1.1.1.1'
l3.dst='2.2.2.2'



# basic ping
sendp(l2/l3/ICMP(), iface='Eth1-1')

# TCP headers and VLAN tagging
sendp(l2/Dot1Q(vlan=750)/l3/TCP(dport=3389), iface='Eth1-51')


# read from PCAP file
pkts = rdpcap('../ping_1_to_3.pcap')
len(pkts)
sendp(pkts[0], iface='Eth1-3')

```

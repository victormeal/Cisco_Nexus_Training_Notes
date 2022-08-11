# PTP Basics

Grand Master Clock
Ordinary Clock
Boundry Clock
Transparent Clock

3 Diferent standards - Many diferences, for example. Some standrads use L2 multicast, others use unicast.

Messages:

Sync - Grand master clock sending the time
Delay Request
Follow up
Delay Response

In multicast - address is 224.0.1.129
For both unicast and multicast - UDP port 319 (event messages) and 320 (general messages)

## TS commands:

```
show ptp counters all
show ptp brief
show system intetrnal ptp info all
show ptp corrections
show ptp clock
show ptp parent
```
```
module level command
show system internal ptplc info all
```

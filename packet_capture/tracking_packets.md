# Tracking Packets / Troubleshooting
```
shr un int vlan 1
  sh ru int vlan 1
  sh ip route 2.2.2.3
  sh ip route 1.1.1.45
  sh ip arp detail  | i 1.1.1.45
  sh run int po 1
  sh port-channel summary interface port-channel 1
  sh port-channel load-balance forwarding-path interface port-channel 1 src-ip 1
.1.1.6 dst-ip  2.2.2.3
  sh cdp neighbors interface e1/45
```
```
sh ip route 2.2.2.3
  sh ip arp detail  | i Vlan3
  sh ip arp detail  | i 3.3.3.1
  sh port-channel summary interface port-channel 3
  sh port-channel load-balance forwarding-path interface port-channel 3  src-ip 
1.1.1.6 dst-ip  2.2.2.3
  sh cdp neighbors  int port-channel  1
  sh cdp neighbors  interface  e1/1
```
```
sh ip roue 1.1.16
  sh ip route 1.1.16
  sh ip route 1.1.1.6
  sh ip arp detail  | i  3.3.3.4
  sh port-channel summary interface port-channel 3
```

```
ip pim send-rp-announce loopback10 route-map RP-Announce scope 10
ip pim send-rp-discovery loopback10 scope 10
ip pim ssm range 232.0.0.0/8
ip pim auto-rp listen
ip msdp originator-id loopback11
ip msdp peer 100.101.0.252 connect-source loopback11
ip msdp mesh-group 100.101.0.252 spoc-msdp
vlan 1

cdp format device-id system-name
spanning-tree pathcost method long
ip prefix-list DENY-PROD-SEGMENTS-TO-NBCUNI seq 100 permit 0.0.0.0/0 le 32 
route-map RP-Announce permit 10
  match ip multicast group 239.3.13.0/24 
route-map RP-Announce permit 20
  match ip multicast group 239.3.14.0/24 
route-map RP-Announce permit 30
  match ip multicast group 239.3.15.0/24 
route-map RP-Announce permit 40
  match ip multicast group 239.3.16.0/24 
route-map RP-Announce permit 50
  match ip multicast group 239.3.17.0/24 
route-map RP-Announce permit 60
  match ip multicast group 239.3.12.0/24 
route-map RP-Announce permit 70
  match ip multicast group 237.3.12.0/24 
route-map RP-Announce permit 80
  match ip multicast group 238.3.12.0/24 
route-map RP-Announce permit 110
  match ip multicast group 239.101.82.0/24 
route-map RP-Announce permit 115
  match ip multicast group 239.101.122.0/24 
route-map RP-Announce permit 120
  match ip multicast group 239.101.182.0/24 
route-map RP-Announce permit 130
  match ip multicast group 239.101.190.0/24 
route-map RP-Announce permit 140
  match ip multicast group 239.101.191.0/24 
route-map RP-Announce permit 170
  match ip multicast group 237.100.101.0/24 
route-map RP-Announce permit 180
  match ip multicast group 238.100.101.0/24 
route-map RP-Announce permit 185
  match ip multicast group 238.127.43.0/24 
route-map RP-Announce permit 190
  match ip multicast group 239.127.43.0/24 
route-map RP-Announce permit 200
  match ip multicast group 239.101.248.0/24 
route-map RP-Announce permit 210
  match ip multicast group 239.101.233.0/24 
route-map RP-Announce permit 220
  match ip multicast group 239.127.27.0/24 
route-map RP-Announce permit 230
  match ip multicast group 239.101.184.0/22 
route-map RP-Announce permit 310
  match ip multicast group 239.127.0.0/16 
route-map RP-Announce permit 320
  match ip multicast group 239.101.242.0/23 
```

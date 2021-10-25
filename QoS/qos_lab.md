

```
ip access-list my_ACL
  10 permit ip 192.168.90.1/32 192.168.90.2/32 
  20 permit ip 192.168.90.2/32 192.168.90.1/32 
class-map type qos match-all my_class
  match access-group name my_ACL
policy-map type qos my_policy
  class my_class
    set dscp 26
interface port-channel4
  switchport
  switchport mode trunk
  switchport trunk allowed vlan 1,26-27,60-61,69,90,97,405,1090
  service-policy type qos input my_policy
  ```

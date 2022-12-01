# shaping

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/qos/configuration/guide/b-cisco-nexus-9000-nx-os-quality-of-service-configuration-guide-93x/b-cisco-nexus-9000-nx-os-quality-of-service-configuration-guide-93x_chapter_01000.html#task_9A3E3CB6A1AF49ED85784C265A41A5B0


```

!! Step 1 - Classification

ip access-list my_acl
  permit ip x.x.x.x/32 any

class-map type qos match-any my_class
  match access-group name my_acl

!! Step 2 - Marking

policy-map type qos my_policy-map_qos
  class my_class
    set qos-group 1


!! Step 3 - Shaping

policy-map type queuing my_policy-map_queueing
  class type queuing c-out-8q-q1
    shape min 0 mbps max 100 mbps


!! Step 4 - Apply policies on the interface/system

!! note this can be applied to the interface or system

interface <ingress port>
  service-policy type qos input my_policy-map_qos


interface <egress port>
  service-policy type queuing output my_policy-map_queueing
```

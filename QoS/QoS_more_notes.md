# QOS

The following are the three types of policies:

Type network qos policy—Defines the characteristics of QoS properties network wide.

Type qos policy —Defines MQC objects that you can use for marking and policing.

Type queuing policy —Defines MQC objects that you can use for queuing and scheduling.

Type Qos policy:

In Type qos policy - You can match on ip, cos, dscp, ACL
And you can set qos group, marking and policing
Basically in type qos policy you are putting into buckets like classifying

Type Queueing Policy:

In Type queueing policy - you will match on qos group
And you set the bandwidth, priority, shaping, WRED, queue-limit, ecn

Type Network Policy:

Classifies the network.
By default the system has a default type qos and type queueing policy applied depending on whether the switch support 4q or 8q architecture
In network qos policy if 4q architecture then 4 qos-groups are present. If 8q then there will be 8 qos-group.
In network qos policy different classes are defined matching different qos groups. Mtu and PFC actions are taken in network qos policy.

```
# show policy-map interface ethernet 1/1


Global statistics status :   enabled

Ethernet1/1

  Service-policy (queuing) output:   default-8q-out-policy 
    SNMP Policy Index:  301991214

    Class-map (queuing):   c-out-8q-q7 (match-any)
      priority level 1
      queue dropped pkts : 0 
      queue depth in bytes : 0 

    Class-map (queuing):   c-out-8q-q6 (match-any)
      bandwidth remaining percent 0
      queue dropped pkts : 0 
      queue depth in bytes : 0 
```
```
# sh queuing summary 

slot  1
=======


Egress Queuing for Ethernet1/1 [System]
------------------------------------------------------------------------------
QoS-Group# Bandwidth% PrioLevel                Shape                   QLimit
                                   Min          Max        Units   
------------------------------------------------------------------------------
      7             -         1           -            -     -            9(D)
      6             0         -           -            -     -            9(D)
      5             0         -           -            -     -            9(D)
      4             0         -           -            -     -            9(D)
      3             0         -           -            -     -            9(D)
      2             0         -           -            -     -            9(D)
      1             0         -           -            -     -            9(D)
      0           100         -           -            -     -            9(D)
el qos group seria el 7
ip access-list iscsi
permit ip 10.10.10.0/24 any
!
class-map type qos match-all class-1
match access-group name iscsi
!
policy-map type qos policy-class-1
class class-1
set qos-group 7
```
```
interface Ethernet1/10
service-policy type qos input policy-class-1
```
```
(config-pmap-que)# system qos
(config-sys-qos)# no   service-policy type queuing output default-8q-out-policy
(config-sys-qos)# service-policy type queuing output default-out-policy
```



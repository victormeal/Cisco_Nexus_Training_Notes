# Steps to apply QoS to traffic with COS==5
```
switch(config)# class-map type qos match-cos5                    <----Create Class map
switch(config-cmap-qos)# match cos 5                             <----Match all traffic with COS 5
switch(config-cmap-qos)# exit

switch(config)# policy-map type qos ingress-marking              <----Create policy-map
switch(config-pmap-qos)# class match-cos5                        <----Use the class map we created
switch(config-pmap-c-qos)# set qos-group 5                       <----Set qos goup 5

switch(config-pmap-c-qos)# int e1/1
switch(config-if)# service-policy type qos input ingress-marking  <--- Aply service policy to the ingress interface
switch(config-if)# exit

On the egress interface:

switch(config)# sh policy-map interface e1/2

Global statistics status :   enabled

Ethernet1/2

Service-policy (queuing) output:   default-8q-out-policy     <----------------------Use to copy
SNMP Policy Index:  301990739

Class-map (queuing):   c-out-8q-q7 (match-any)
  priority level 1
 queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q6 (match-any)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q5 (match-any)                 <--We will use this (qos5)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q4 (match-any)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q3 (match-any)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q2 (match-any)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q1 (match-any)
  bandwidth remaining percent 0
  queue dropped pkts : 0
  queue depth in bytes : 0

Class-map (queuing):   c-out-8q-q-default (match-any)
  bandwidth remaining percent 100
  queue dropped pkts : 0
  queue depth in bytes : 0
switch(config)# qos copy policy-map type queuing  default-8q-out-policy prefix TAC-5           <-----Copy the policy map
switch(config)# policy-map type queuing TAC-58q-out
switch(config-pmap-que)# CLass type queuing c-out-8q-q-default
switch(config-pmap-c-que)# bandwidth remaining percent 50                                      <-----Remove bandwith to de default class
switch(config-pmap-c-que)# CLass type queuing  c-out-8q-q5
switch(config-pmap-c-que)# bandwidth remaining percent 50                                      <-----Set the bandwith utilization on qos5

switch(config-pmap-c-que)# int e1/2                                                            <--Go to the egress interface
switch(config-if)# service-policy type queuing output TAC-58q-out                              <--Apply the service policy

Verify the config:

switch(config-if)# sh run int e1/1-2

!Command: show running-config interface Ethernet1/1-2
!Running configuration last done at: Wed Apr 24 22:15:01 2019
!Time: Wed Apr 24 22:15:06 2019

version 9.2(2) Bios:version 07.65

interface Ethernet1/1
service-policy type qos input ingress-marking
no shutdown

interface Ethernet1/2
service-policy type queuing output TAC-58q-out
no shutdown
```

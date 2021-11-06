# QoS

https://techzone.cisco.com/t5/Nexus-9500/Nexus-9k-Landing-Page/ta-p/747089

https://techzone.cisco.com/t5/Nexus-9500/Nexus-9500-Output-Discard-Troubleshooting/ta-p/1028384

- todo cae en el grupo 0
- dependiendo del nexus tienes diferente cant de grupos
- siemplre se aplica al trafico de salida (egress), excepto los de 7k, ahi es de ingress (raro)
```
sh queuing int e1/1

sh policy-map interface e1/1

show policy-map int e1/1
```


- burst traffic

```
hardware qos 

sh int e1/1 counters erros
! ver outDiscards, esto lo ms posible es por burst traffic drops
sh queuing int e1/1
! lo visto antes se puede comprobar aqui
```

```
clear counters interface?
```
----

+ QOS only kicks in when there is congestion
+ IF the interface utilization is maxed out, then the respective QOS allocations of BW apply. Otherwise a group /queue can exceed the value (under no congestion)
----
+ Everything in QOS-group 1 -- I can send 100% of traffic that matches the Qos-group 1 classification and not see drops (provided I don't exceed the interface utilization)
+ 50% in QOS-group 1 + 40% in QOS-group 0 is still fine (I am below 100% instantaneous utilization = no congestion)

+ please note that congestion is an instantaneous criterion (well almost instantaneous) = can't rely on the 30s average rate of the interface
----
+ If you have a mix of "important" and "not as important" traffic... 
QOS allows you to set a GUARANTEED (minimum) throughput under congestion
----
+ The other thing to know is that ANYTHING that goes into the PRIORITY QUEUE will take precendence = under congestion, the only queue that gets services is the priority Queue (no matter what BW allocation you associate to any other queue)
----
+ Nexus uses a "strict priority" model.
----
+ In summary....

No priority Queue
qos-group 0 -- you can associate BW0 30% (minimum guaranteed)

qos-group 1 -- you can associate BW1 70% (minimum guaranteed)

(BW1 + BW0 = 100%).

If there is congestion on the interface then Qos-group 1 will see AT LEAST 70% throughput If there is 30% of traffic in QOS-group 0.. then any ADDITIONAL traffic in QOS-Group 1 WILL BE DROPPED
----

The one caveat I want to stress for the latter is this.... if - for whatever reason - the customer sends 100% of priority queue traffic through the switch for an extended period of time.... then NOTHING else will go through the box.... so ... choose carefully

----
"Extra" reading .... WRED
instead of waiting for the interface to be congested and start dropping 100% of the EXCESS traffic, another approach (complementary) is to use WRED (Weight Random Early Detection) for TCP traffic

The idea of WRED is that we drop a little bit (more) as the utilization increases... this is suited for TCP long flows because TCP fast retransmission can recover by detecting the drop (lack of ACK) and retransmit only the missing packets. For short TCP flows (less than 3 packets), usually we don't want to drop anything because TCP fast retransmission is not possible.

based on utilization thresholds

queue utilization < t1 -- do nothing
t1 < queue utilization < t2 -- start dropping with a linear rate (drop more as the utilization increases)
t2 < queue utilization -- drop 100% of excess traffic (same as Tail Drop)
----

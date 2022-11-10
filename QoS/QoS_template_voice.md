# QoS_template_voice

```
Here’s a Voice QoS template for the Nexus 9300:
 
1.) Create class-map type qos and match on CoS/DCSP/ACL. I will match DSCP 46 in my example:
 
class-map type qos match-any Voice-Traffic
  match dscp 46
 
2.) Create policy-map type qos and set qos-group 3. By defualt c-out-q3 (qos-group 3) is already configured as the priority queue.
 
When configuring priority for one class map queue (SPQ), you need to configure the priority for QoS group 3. When configuring priority for more than one class map queue, you need to configure the priority on the higher numbered QoS groups. In addition, the QoS groups need to be adjacent to each other. For example, if you want to have two SPQs, you have to configure the priority on QoS group 3 and on QoS group 2.
 
policy-map type qos PQ-Classification
  class Voice-Traffic
    set qos-group 3
 
3.) Attach the policy map to use as the service policy for the desired interfaces or VLAN. You can classify traffic on Layer 2 ports based on either the port policy or VLAN policy of the incoming packet but not both. If both are present, the device acts on the port policy and ignores the VLAN policy.
 
interface Ethernet1/5
  service-policy type qos input PQ-Classification
 
or
 
vlan configuration 10
 service-policy type qos input PQ-Classification
 
To verify that the traffic is being classified correctly enter "show queuing interface ethernet  x/y"  for the egress interface and confirm the packets are being hitting the priority queue QoS group 3.
 
N9K# show queuing interface ethernet 1/1 | begin "QOS GROUP 3"
|                              QOS GROUP 3         |
+---------------------------------------------------------+
|                       |  Unicast       |Multicast       |
+---------------------------------------------------------+
|               Tx Pkts |         2763095|               0|
|               Tx Byts |       364728540|               0|
| WRED & Tail Drop Pkts |               0|               0|
| WRED & Tail Drop Byts |               0|               0|
|          Q Depth Byts |               0|               0|
+--------------------------------------------------------++
 
The QoS Group is the default system QoS group used for this, you can have 4QoS groups or 8 QoS group, depending on the line card you have.
 
When you configure QoS features, and the system requests MQC objects, you can use system-defined MQC objects for 4q mode or system-defined objects for 8q mode.
On the Cisco Nexus device, a system class is uniquely identified by a qos-group value. A total of four system classes are supported. The device supports one default class which is always present on the device. Up to three additional system classes can be created by the administrator. Only egress queuing, network-qos, and type qos for FEX policies are supported on the system QoS target.
Default System Classes
The device provides the following system classes:
•	Drop system class
By default, the software classifies all unicast and multicast Ethernet traffic into the default drop system class. This class is identified by qos-group 0.
 
This is a lot of information, and there are several customization classes you can use, that will depends on your needs, i strongly recommend to you to review our cisco nexus 9000 QoS configuration guide for this:
 
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/qos/configuration/guide/b_Cisco_Nexus_9000_Series_NX-OS_Quality_of_Service_Configuration_Guide_7x/b_Cisco_Nexus_9000_Series_NX-OS_Quality_of_Service_Configuration_Guide_7x_chapter_011.html#concept_1878FA466E3B42A9831E28518F3C558D
 
for a general Example:
 
class-map type qos match-any Voice-Traffic
  match dscp 46
 
policy-map type qos PQ-Classification
  class Voice-Traffic
    set qos-group 1 ----- system defined QoS groups
 
interface Ethernet1/5
  service-policy type qos input PQ-Classification
 
Now for the Bandwidth assignment:
 
policy-map type queuing QUEUING_POLICY
  class type queuing c-out-q1
    bandwidth percent 10 --------------------- 10% of Bandwidth assigned to QoS Group 1
  class type queuing c-out-q2
    bandwidth percent 15
  class type queuing c-out-q3
    priority level 1 -------------- priority Queue
  class type queuing c-out-q-default
    bandwidth percent 75
 
This a general configuration for voice traffic, you can use it to customize your needs.
```

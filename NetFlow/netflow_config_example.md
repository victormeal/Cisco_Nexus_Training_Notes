# netflow_config_example.md
```
+ Lab testing

Model:N9K-C93180YC-FX
NXOS:7.0(3)I7(8)      >>> customer is using 7.0(3)I7(3) 

+ Config

N9K-2# show run netflow 

!Command: show running-config netflow
!Running configuration last done at: Thu Jan 27 05:24:33 2022
!Time: Thu Jan 27 05:27:24 2022

version 7.0(3)I7(8) Bios:version 05.45 
feature netflow

flow exporter NetFlow-to-WhatsUp
  destination 1.1.1.1
  transport udp 9999
  source Vlan3
  version 9
    template data timeout 60
flow record Netflow-Record
  match ipv4 source address
  match ipv4 destination address
  match ip protocol
  match ip tos
  match transport source-port
  match transport destination-port
  collect transport tcp flags
  collect counter bytes long
  collect counter packets long
  collect timestamp sys-uptime last
  collect ip version
flow monitor NetFlow-Monitor
  record Netflow-Record
  exporter NetFlow-to-WhatsUp


interface Vlan1
  ip flow monitor NetFlow-Monitor input  

interface Vlan2
  ip flow monitor NetFlow-Monitor input  

N9K-2# 


+ Getting Flows

N9K-2# show flow exporter 
Flow timeout 10 
Flow exporter NetFlow-to-WhatsUp:
    Destination: 1.1.1.1
    VRF: default (1)
    Destination UDP Port 9999
    Source Interface Vlan3 (192.168.3.2)
    Export Version 9
        Sequence number 16
        Data template timeout 60 seconds
    Exporter Statistics
        Number of Flow Records Exported 37
        Number of Templates Exported 4
        Number of Export Packets Sent 17
        Number of Export Bytes Sent 2356
        Number of Destination Unreachable Events 0
        Number of No Buffer Events 0
        Number of Packets Dropped (No Route to Host) 0
        Number of Packets Dropped (other) 0
        Number of Packets Dropped (LC to RP Error) 0
        Number of Packets Dropped (Output Drops) 0
        Time statistics were last cleared: Never

Feature Prio: Netflow
N9K-2# show flow cache 
IPV4 Entries
SIP              DIP              BD ID    S-Port   D-Port   Protocol  Byte Count        Packet Count      TCP FLAGS    TOS     if_id       flowStart        flowEnd
1.1.1.1          192.168.3.2      1        771      0        1         74                1                 0x0          0x0     0x9010001   684341           684591 
 


N9K-2# 
```

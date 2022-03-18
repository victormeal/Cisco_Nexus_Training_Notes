# IP SLA ICMP echo

```
F241.01.24-N7K-C7004-text(config)# show run sla sender 

!Command: show running-config sla sender
!Time: Fri Mar 18 07:54:57 2022

version 8.2(1)
feature sla sender

ip sla 10
  icmp-echo 1.1.1.1 source-ip 3.3.3.3
    frequency 5
ip sla schedule 10 life forever start-time now
ip sla reaction-configuration 10 react timeout threshold-type immediate action-type trapOnly
ip sla logging traps


F241.01.24-N7K-C7004-text(config)# 

F241.01.24-N7K-C7004-text(config)# show logging logfile 
2022 Mar 18 07:51:13 F241.01.24-N7K-C7004-text %SYSLOG-1-SYSTEM_MSG : Logging logfile (messages) cleared by user
2022 Mar 18 07:53:05 F241.01.24-N7K-C7004-text %SLA_SENDER-3-SNMP: rttMonCtrlAdminTag = (null)
2022 Mar 18 07:53:05 F241.01.24-N7K-C7004-text %SLA_SENDER-3-IPSLATHRESHOLD: IP SLAs(10): Threshold Occurred for timeout
2022 Mar 18 07:53:20 F241.01.24-N7K-C7004-text %SLA_SENDER-3-SNMP: rttMonCtrlAdminTag = (null)
2022 Mar 18 07:53:20 F241.01.24-N7K-C7004-text %SLA_SENDER-3-IPSLATHRESHOLD: IP SLAs(10): Threshold Cleared for timeout
F241.01.24-N7K-C7004-text(config)# 

F241.01.24-N7K-C7004-text(config)# show ip sla statistics 

IPSLAs Latest Operation Statistics

IPSLA operation id: 10
        Latest RTT: 1 milliseconds
Latest operation start time: 07:54:35 UTC Fri Mar 18 2022
Latest operation return code: OK
Number of successes: 30
Number of failures: 2
Operation time to live: forever
F241.01.24-N7K-C7004-text(config)# 


==========

F241.01.24-N7K-C7004-text(config)# show run sla sender 

!Command: show running-config sla sender
!Time: Fri Mar 18 08:01:26 2022

version 8.2(1)
feature sla sender

ip sla 10
  icmp-echo 1.1.1.1 source-ip 3.3.3.3
    threshold 200
    timeout 200
    frequency 1
ip sla schedule 10 life forever start-time now
ip sla reaction-configuration 10 react timeout threshold-type immediate action-type trapOnly
ip sla logging traps


F241.01.24-N7K-C7004-text(config)# 
```

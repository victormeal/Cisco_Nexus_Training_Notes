# SPAN to CPU + VACL

Step1. - Configure the monitor session (SPAN to CPU)
```
#conf t
(config)# no monitor session 1
(config)# monitor session 1
(config-monitor)#destination interface sup-eth0
(config-monitor)# source interface <interface>     <<<<< here you need to specify the interface (ethernet or port-channel where the traffic is passing through)
(config-monitor)# no shut
```
Step2. - Setting up the capture and the filters
```
# ethanalyzer local interface inband mirror capture-filter "src host <x.x.x.x> and dst host <y.y.y.y>" limit-captured-frames 0
```
Other filter that you can use is:
```
# ethanalyzer local interface inband mirror capture-filter "host <x.x.x.x> && host <y.y.y.y>" limit-captured-frames 0
```
If you are not seeing all the packets being capture is possible that to prevent to overload the CPU some of the packets are not being redirected. We can try to limit the captures with an ACL on the monitor session. This can only be applied in the Rx direction. Configuration example:
```
(config)# ip access-list SPAN
(config-acl)#permit ip x.x.x.x 0.0.0.0 y.y.y.y 0.0.0.0
(config-acl)#exit
(config)#vlan access-map span_filter
(config-access-map)# match ip address SPAN
(config-access-map)# action forward
(config-access-map)# exit
(config)# monitor session 1
(config-monitor)# no source interface <interface>
(config-monitor)# source interface <interface> rx
(config-monitor)# filter access-group span_filter
(config-monitor)# exit
```
If you want to save the capture to then retrieve the file from the bootflash of the device and visualize it in wireshark you can use the following command when setting up the capture:
```
# ethanalyzer local interface inband mirror capture-filter "src host <x.x.x.x> and dst host <y.y.y.y>" limit-captured-frames 0 write bootflash:<file_name>.pcap
```
Let me know if you have any questions or comments.
Regards.


```
ip access-list SPAN-to-CPU-ACL
permit ip host 10.127.94.112 host 172.17.1.138
permit ip host 172.17.1.138  host 10.127.94.112


exit

vlan access-map SPAN-to-CPU-filter
match ip address SPAN-to-CPU-ACL
action forward

exit

monitor session 10
filter access-group SPAN-to-CPU-filter
```


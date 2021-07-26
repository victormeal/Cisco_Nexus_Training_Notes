
-----
PACL (port access control list) (L2 port)
- solo de ingress
```
ip access-list tac
permit ip 1.1.1.2/32 1.1.1.1/32
permit ip any any
statistics per-entry

int e1/4
ip port access-group tac in

sh ip access-list tac
```
--------
RACL (routed access control list) (L3 interface, SVI)
- ingress y egress **
```
ip access-list tac
permit ip 1.1.1.2/32 2.2.2.1/32
permit ip any any
statistics per-entry

int vlan 1
ip access-group tac in
```
-------
SPAN CPU
mandar trafico de una interfaz al cpu
solo en 9k taho ex, fx, C

```
sh module
monitor session 1
source interface e1/1
destination int sup-eth 0
no sh
exit

ethanalyzer local interface inband mirror display-filter icmp limit-captured-frames 0
```
no olvidar apagar o quietar el monitor session creado
no veremos el trafico creado de ese nexus, no es siempre
https://techzone.cisco.com/t5/Nexus-9300/Nexus-9000-Cloud-Scale-ASIC-NX-OS-SPAN-to-CPU-Procedure/ta-p/1449262

50 kbps Default Hardware Rate Limiter
 

By default, Cisco Nexus 9000 series switches limit the rate of traffic replicated to the control plane through a SPAN-to-CPU monitor session to 50 kbps. This rate limiting is performed at the Cloud Scale ASIC/forwarding engine and is a self-protection mechanism to ensure the control plane of the device is not overwhelmed with replicated traffic.
```
show hardware rate-limiter span 
hardware rate-limiter span 250 
```

----

```
clear ip access-list counters myACL
```

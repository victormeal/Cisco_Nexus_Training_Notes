# NetFlow

- Network monitoring
- ingress ip packets
- keys -> flow record -> flow
- all keys need to match
- flow exporter -> gather data -> Netflow controller
- exports -> periodically or manual
- Flow Monitor = flow record + flow exporter
	- its applied to an interface

- L2 NetFlow keys

## Configuring NetFlow
1. Enable the NetFlow feature.	
2. Define a flow record by specifying keys and fields to the flow.
3. Define an optional flow exporter by specifying the export format, protocol, destination, and other parameters.
4. Define a flow monitor based on the flow record and flow exporter.
5. Apply the flow monitor to a source interface, subinterface, or VLAN interface.

```
feature netflow
```
```
flow record <name>
match <ipv4 destination address>
collect <counter packets>
```
```
show flow record <netflow protocol-port> 
```
```
flow exporter <name>
destination a.b.c.d
source ethernet 1/1
```


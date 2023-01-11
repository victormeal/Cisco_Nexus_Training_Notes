# link_flap_commands

```
Sh mod
Sh run
Sh interface e <x/y>
Sh interface e <x/y> trans det
Sh logging
Sh system internal ethpm event-history interface e <x/y>
show pie interface ethernet<x/y> link-flap-rca detail
show pie interface ethernet3/9 transceiver-insights detail
slot 1 quoted “show hardware internal tah link-events fp-port <y>“
slot 1 quoted “show system internal port-client link-event”
slot 1 quoted “show hardware internal tah event-history front-port <y>”
```
juan commands

```
Sh version
Sh module
Sh run
Sh vpc (if applicable)
Sh ip int br vrf all
Sh int status
sh interface transceiver detail
sh interface
sh system internal ethpm event-history interface ex/y <<<<<replace x and y with the interface in question
attach module X <<<<<Replace X with the number of module example from int e1/2 X will be equal to 1
sh hardware internal tah event-history front port Y <<<<<<<Replace Y with the number of front port example from int e1/2 Y will be equal to 2
Sh log
Sh log nvram
Sh tech (If possible)
```
felipe info
```
Analysis for Link Flap 
=======
N9k // 
 
+ For the two events we still have port-client logs for, we see that the link went down because we lose light on the link:
 
pib1-r1r4-cs9332a# attach module 1
module-1# show system internal port-client event-history port 49 <——— Take the interface number and multiply by 4 then subtract 3.
<snip>
98) Event E_PC_LOG_IF_DN length:27, at 685444 usecs after Wed Apr 26 23:30:48 2017
  Ethernet1/13(0x1a001800) Link status:DOWN(0x3) 
  Stsinfo:Link down debounce timer stopped and link is down(0x40e50007) <--------------------- Debounce timer has lapsed and link is down.
 
 
99) Event E_PC_LOG_IF_CMD length:8, at 684844 usecs after Wed Apr 26 23:30:48 2017
  Cmd:PORT_CMD_DISABLE(0x3) Status:SUCCESS(0x0)
  
 
100) Event E_PC_LOG_IF_DN_START length:27, at 585814 usecs after Wed Apr 26 23:30:48 2017
  Ethernet1/13(0x1a001800) Link status:DOWN(0x3) 
  Stsinfo:Link down debounce timer started(0x40e50006) <--------------------- First event in port-client for port 13 indicating we lost light on the link.
 
 
101) Event E_PC_LOG_IF_CMD length:8, at 671542 usecs after Mon Apr 24 13:07:12 2017 <--------------------- Last event on 24 Apr.
  Cmd:PORT_CMD_EDGE_FORCE_TIMEOUT(0x6b) Status:SUCCESS(0x0)
  
<snip> 
  
145) Event E_PC_LOG_IF_DN length:27, at 217783 usecs after Mon Apr 24 13:07:08 2017
  Ethernet1/13(0x1a001800) Link status:DOWN(0x3) 
  Stsinfo:Link down debounce timer stopped and link is down(0x40e50007) <--------------------- Debounce timer has lapsed and link is down.
 
 
146) Event E_PC_LOG_IF_CMD length:8, at 217274 usecs after Mon Apr 24 13:07:08 2017
  Cmd:PORT_CMD_DISABLE(0x3) Status:SUCCESS(0x0)
  
 
147) Event E_PC_LOG_IF_DN_START length:27, at 108264 usecs after Mon Apr 24 13:07:08 2017
  Ethernet1/13(0x1a001800) Link status:DOWN(0x3) 
  Stsinfo:Link down debounce timer started(0x40e50006) <--------------------- First event in port-client for port 13 indicating we lost light on the link.
 

```

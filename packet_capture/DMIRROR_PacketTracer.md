# Packet Tracer
- solo en 9k broadcom (PX,PQ,P,Q)
- poca info

```
test packet-tracer src-ip 1.1.1.2 dest-ip 1.1.1.1
test packet-tracer start
test packet-tracer show

test packet-tracer stop
test packet-tracer clear filter 1
```



# DMIRROR
- solo en 9k broadcom (PX,PQ,P,Q)
- es posible que no veas el trafico porque el CPU lo tire (CoPP)

```
pktmgr internal span-drop enable

sh system internal ethpm info interface e1/7 | i dpid
! nxos_port=6

bcm-shell module 1
dmirror xe6 destport=cpu0 mode=all

! para revisar
dmirror show

exit

ethanalyzer local interface inband mirror display-filter icmp limit-c 0
! paquetes duplicados
```
```
bcm-shell module 1
dmirror xe6 mode=off
```

```
sh system internal ethpm info global | i i nxos_port=6
```

## SPAN
```
monitor session 1
source nt e1/1
destination int e1/2
no sh
exit
bcm mod 1


show run monitor

int e1/8
no sh
sw monitor
.
.
.
```
basicamente con `dmirror show` puedes ver sesiones de span, no se debe apagar el span desde aqui



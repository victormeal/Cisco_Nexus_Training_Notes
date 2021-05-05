# FEX (fabric extender)

- extender los puertos del nexus
- dispositivo aparte (nexus 2000)
- NIF (network interface) uplink - dedicados
- HIF (host interface)
- dump switches: no pueden hacer forwarding del trafico, tienen que mandarlo al N9k
- no puedes conectarte (consola/terminal) a un FEX
- no conectar L2 (switches) a los puertos del FEX, purso Host L3 (servidores best practice)
- si conectas otro switch la interfaz se ira a BPDUerrDisable
- no distinguen trafico de control y normal
- solo se puede hacer SPAN (ingress) y ACLs para capturas

## config
```
install feature-set fex
feature-set fex

int e1/53
sw
channel-group 1
exit
int po 1
sw
sw mode fex-fabric
! del 101-199, para diferenciar entre cada fex, es libre
fex associate 101
no sh
```
esperar a que el image download -> debe de estar online
```
show fex
show fex 101 detail
which | i i fex

attach fex 101
sh system internal log sysmgr...

! ver procesos atorados
sh sytem internal mts buffers summary
! 284 es de ssh
sh system mts sup sap 284 description
sh sytem internal mts buffers details
```
e101/1/x -> asi se veria la interface en el nexus

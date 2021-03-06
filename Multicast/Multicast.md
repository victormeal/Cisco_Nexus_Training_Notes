# Multicast

## Nexus notes
- En nexus solo soporta sparse mode
- para troubleshootear multicast es importante armar la topologia
- Terminology
  - Sender, Receiver
  - First-hop Router (FHR), Last-hop Router (LHR)
  - RP
- todos los routers en el camino tienen que tener el (S,G)
- si no tienes el (S,G) es que no te esta llegando el trafico de multicast
- el (S,G) se aprende cuando llega el trafico de multicast
- si no tienes un (*,G) significa que no tienens a nadie subscrito.
- Si unicast esta roto, multicast esta roto. Pero puede estar bien unicast pero mal multicast.
- limitaciones en RACL, (por ejemplo no bloquea trafico, pero PACL si)
- en VPC solo un nexus hace forwarding hacia el receiber. En ambos veremos el S,G pero uno no tenra interfaz en el OIL y el otro si.
- En la config guide de pim esta escrito que no se sportan pim adyacencies over vpc vlans
----
## Multicast Theroy
- Multicast group -> Class D Address Range (224.0. 0.0 – 239.255. 255.255)
- Multicast MAC address -> usa los ultimos 3 octetos, el bit mas a la izquierda se hace cero y se pasa a HEX. Esto conforma la segunda mitad. La primera mitad siempre es 01-00-005e
- debido a que el bit mas a la izuierda siempre se hace 0, puede haber MAC address iguales para dos grupos distintos.

## IGMP (Internet Group Management Protocol)
Manera en el que los host le dicen a su router que quiere pertenecer a un grupo.
- IGMPv1 --> el router le pregunta cada 60s al host, si quiere pretenecer todavia a ese grupo.
- IGMPv2 --> lo mismo pero ahora el host puede decir en cualquier momento que quiere abandonar el grupo.
- IGMPv3 --> lo mismo pero el host puede decir que quiere trafico de un source en particular.
- IGMP Snooping --> manera en que un switch sabe que un host pidio unirse a un grupo y cunado reciba la MAC para ese grupo mandarsela. Si no esta activado un switch manda los frames de multicast como si fuera broadcast.

## RPF (Reverse Path Forwarding)
Si el source manda multicast traffic y tiene varios caminos, este va tomar todos pero al juntarse de nuevo podria ocasionar que el destination le llegaran paquetes duplicados. Para evitar esto RPF, solo acpta trafico multicast de la interfaz que usaria para mandar trafico unicast al source.


## PIM (Protocol Independet Multicast)
- no importa si es ip o ipv6
- podriamos pensarlo que es el protocolo de ruteo de Multicast
### PIM-DM (Dense Mode) - Source Distribution Tree
Manda el trafico por TODA la topologia, y eventualmente determina el mejor camino con:
- Prune msg: mensaje que manda un router para no recibir trafico multicast por esa interfaz.
- Graft msg: mensaje que manda un router para volver a recibir trafico multicast por esa interfaz.
- Graft-ACK: respuesta al Graft
### PIM-SM (Sparse Mode) - Shared Distribution Tree
Evita mandar el trafico a toda la topologia para converger.
- RP (Rendezvous Point)
- Host ----> IGMP Report msg ----> Last-Hop Router ----> (*,G) Join msg ----> RP <---- (S,G) <---- FHR <---- Server
  - (source,group) * = doesn't matter de source
  - the FHR->RP->LHR path is named shared path tree
  - the FHR->LHR path is named shortest path tree
- Lo anterior puede ser un ruteo suboptimo por lo que cuando el Last-hop router recibe el paquete se da cuenta de que hay un mejor camino y manda un (S,G) Join msg directo al source router. Sin embargo, ahora recibiria doble paquete, por lo que el last-hop router manda un Prune msg para no recibir el paquete por el camino sub optimo. Esto se llama Shortest Path Tree Switchover
### PIM Sparse-Dense Mode
- si puede usa sparse, si no usa dense.
- util para no configurar el RP en cada dispositivo. Utiliza dense mode para encontrar al RP y luego cambiar a sparse.

----

## IOS Conifg
- Comandos para habilitar una interfaz para ser multicast
```
ip multicast-routing
int e0/0
ip pim dense-mode ---> ip pim sparse-mode ---> ip pim sparse-dense-mode
```
- Decirle en donde esta el RP, manera manual
```
ip pim rp-address x.x.x.x ---> RP
```
- Comandos para habilitar una interfaz para unirse a un grupo de multicast, como si fuera un host
```
int e0/0
ip igmp join-group x.x.x.x
```
- Comandos para habilitar que busque el RP, manera dinamica.
```
!no ip pim rp-address x.x.x.x
! Mapping agent
ip pim send-rp-announce loopback 0 scope ?   ---> TTL?
ip pim send-rp-discovery scope ?

```
- Comandos para habilitar una igmp snooping
```
ip igmp snooping
```
```
show ip mroute
sh ip pim rp
sh ip pim internal vpc rpf-source 

sh ip igmp snopping
```


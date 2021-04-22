# ELAM on Nexus 7k
- buscar guia especifca

- VDC (virtual device context) only on nexus 7k
- admin VDC
- ethanalyzer only can run on admin vdc
- `show vdc`
- `switchto vdc vdc2`
- linecards M (mas features, L3)
- linecards F (menos features, no routing)

- usar el elam en el admin vdc

## esto es para las linecards M

- e1/16

```show module```
```attach module 1``` -> para ver que tipo de linecard (M,F)
```show hardware internal dev-port-map```
L2LKP
eureka -> version name
L2LKP + 1 -> 1
```elam asic eureka instance 1```
dbus - rbus (ingress - egress)
```trigger dbus dbi ingress ipv4 if ?```
```trigger dbus dbi ingress ipv4 if souce-ipv4-address x.x.x.x destination-ipv4-address y.y.y.y rbi-corelate```
```trigger rbus rbi pb1 ip if cap2 1``` -> este comando no cambia
```start```
```status``` -> solo te dice si se triggero, no te muestra el paquete
```show dbus | i seq```
```sh rbus | i seq```
el seq # debe de hacer match
para ver el paquete:
```show dbus```
`show rbus`
ver los indexes (source_index, dest_index), decodificar en el vdc que le coresponde esa interfaz

## es distinto para las Fs


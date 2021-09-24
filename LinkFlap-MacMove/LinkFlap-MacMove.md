# Link-Flap
https://techzone.cisco.com/t5/Hardware/Nexus-3000-Link-Flaps-ETHPM-and-ETHPC-logs/ta-p/865892
- problemas de capa 1 o de config

- cambiar el cable, puerto,hacer prueba de loop
- link loop, conectar un puerto del nexus a otro de ese mismo nexus, para probar el cable

- revisar de hardware a control plane (asic,port-client-ethpm)

- revisar transciver, debe de ser de Cisco
```
sh int e1/1 transceiver det
```
- buscar nexus y sus transcivers:https://tmgmatrix.cisco.com/

----
- ASIC
```
attach mod 1
! para broadcom el asic es bcm
sh hardware internal tah event-history front-port 1
!! ordenados del mas nuevo (1) al mas viejo
```
----
- port-client

attach mod 1
sh system internal port-client | i 1/1


debounce timer -> 100ms
The port debounce time is the amount of time that an interface waits to notify the supervisor of a link going down.

----
- ethpm
```
sh system internal ethpm event-history fornt-port 1
sh system internal ethpm event-history interface e1/1
! ordenados del mas antiguo (1) al mas nuevo
```
----
normalmente pedir estos comandos
```
sh tech det
sh tech usd-all
sh tech ethpm
```

----

# MAC move/flap

- aprender una MAC por distintas int (loops)
- siempre se debe aprender una misma MAC por solo una interfaz
- sintoma de que hay loops
- determinar por cual int si debes aprender esa MAC y tshoot las otras
 - hacer tracking hop by hop, de donde la estas aprendiendo
- proceso que instala las MAC en la MAC address-table MTM y despues L2FM
```
show mac address-table
```

-te dice porque int esta moviendose la MAC
-puede cambiar el nombre del proceso, (l2gm|fwm)
```
logging level l2fm 6
mac address-table notification mac-move
sh logg | i i mac
sh logg | i i l2fm
```
```
sh hardware mac address-table 1
```
```
sh system internal l2fm l2dbg macdb address xxxx.xxxx.xxxx
! index de la interfax (if/swid)
sh interface snmp-ifindex | 0x1axxxxx
```
- si el nexus detecta 10 MAC move en 1s, el nexus deja de aprnder MAC en ese vlan, es un sistema de proteccion, se deshabilita por 120s



 

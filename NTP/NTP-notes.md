# NTP
- un core se synchroniza con el server y hacia abajo el core synchroniza tu red
- por cada paquete que envie un cliente el server tiene que mandar, si no no se synchroniza
- rate 1:1, tienes que resivir el paquete de regreso basante rapido
- stratum 1, conectado al server. Stratum de 16 unreachable.
- ver si alcanzas al NTP server, puede que haya firewalls y bloque icmp
- tiempo en el que syncroniza con el server (relativamente poco), tiempo en el que se actualiza el tiempo en el nexus indeterminado (puede tardar)


```
feature ntp
ntp server 1.1.1.1
sh ntp peer-status
sh nto statistics peer ipaddr 1.1.1.1
ethanalyzer local interface inband display-filter ntp limit-captured-frames 0
```

# QoS

https://techzone.cisco.com/t5/Nexus-9500/Nexus-9k-Landing-Page/ta-p/747089

https://techzone.cisco.com/t5/Nexus-9500/Nexus-9500-Output-Discard-Troubleshooting/ta-p/1028384

- todo cae en el grupo 0
- dependiendo del nexus tienes diferente cant de grupos
- siemplre se aplica al trafico de salida (egress), excepto los de 7k, ahi es de ingress (raro)
```
sh queuing int e1/1

sh policy-map interface e1/1

show policy-map int e1/1
```


- burst traffic

```
hardware qos 

sh int e1/1 counters erros
! ver outDiscards, esto lo ms posible es por burst traffic drops
sh queuing int e1/1
! lo visto antes se puede comprobar aqui
```

```
clear counters interface?
```

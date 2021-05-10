# vPC Features

## peer switch
- avoid STP problems
- both switches send BPDUs
- Nexus needs to be the root
- inside the domain `peer-switch`
- usa la system-mac para el root, y ambos switches mandan bpdus
- solo usar esto si los vpc son los roots

## Bridge assurance
- port type edge == port fast
- esperar recibir BPDUs del vecino
´spann port type network´
- utilizar solo de nexus a nexus
- habilitarlo de ambos lados
- manda BPDUs y  espera recibir, si no bloquea la interfaz

show pann vlan 2
show spann int e1/6 detail



ethanalyzer local interface inband display-filter STP limit-captured-frames 20

show mac address-table address



## peer gateway
`peer-gateway`
- se usa en HSRP
- modifica la regla de loop free path (la S)



sh cli hist un

feaute hsrp
int vlan 1
hsrp v2
hsrp 1
ip x.x.x.x


## L3 peer-router

`layer3 peer-router`

# ELAM
https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/213848-nexus-9000-cloud-scale-asic-tahoe-nx-o.html


691236816

## ASIC
 terminan con ...EX/FX (y otros) -> "taho"
  elam - 5k,7k, 9k(taho) 
   - solo catura 1 paquete
   - saber IP, MAC source y en que interfaz (ingress mejor)
   - Para saber en que ASIC y en que slice esta: `show hardware internal tah interface e1/49`
    - cosas importantes que ver: ASIC, SrcID, Slice
   - `attach mod 1`
`debug platform internal tah elam asic 0`
`trigger init asic 0 slice 1 lu-a2d 1 in-select 6|7 out-select 0 use-src-id 8`
 puedes cambiar el 6 (7) y no usar use-src-id 8
`reset`
`set outer ipv4|arp|l2 dst_ip x.x.x.x src_ip x.x.x.x`
`start`
`report`

`sh hardware internal tah niv_idx 0x5bf`
! te dice porque int segun el index
sh system internal ethpm info all | i dpid=16


## elam wrapper


`debuf platform internal tah elam`
`trigger init`
`reset`
`set outer ipv4|arp|l2 dst_ip x.x.x.x src_ip x.x.x.x`
`start`
`report`



## terminan con ...PX/QX? -> "broadcom"
  dmirror

ASIC
 Slice

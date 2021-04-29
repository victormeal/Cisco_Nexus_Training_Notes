# TCAM

https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/119032-nexus9k-tcam-00.html
da espacio de memoria a...
- ACL
- QoS
- SPAN
- CoPP
- NAT

- dividida en espacios de 256 bytes
- divididos en instancias (0-1) siempre fijarte en la 0


```
show systrem internal access-list globals
sh logg log | i i tcam
```

# TCAM Carving 
- tomar espacio de un feature y darselo al que queremos ocupar

```
show hardware access-list resource region
show hardware access-list resource region | i i ing

show hardware access-list resource utilization

hardware access-list tcam region ing-racl 1536

hardware access-list tcam region ing-ifacl 256




```

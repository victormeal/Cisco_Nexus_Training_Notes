
# GRE Tunnel
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/6-x/interfaces/configuration/guide/b_Cisco_Nexus_9000_Series_NX-OS_Interfaces_Configuration_Guide/b_Cisco_Nexus_9000_Series_NX-OS_Interfaces_Configuration_Guide_chapter_01000.html
- encapsular ip en ip
- outer header ---> GRE Header ---> inner header
- revisar que las interfaces fisicas se puedan alcanzar (underlay)
- en Nexus, un tunnel la marca up si la interfaz fisica esta up y el tunnel esta up (no shut)
- limitacion en vpc, no podemos usar source/destination una interfaz que sea vpc. No se puede usar vpc
----
- IP tunnels consists of the following three main components:
  - Passenger protocol—The protocol that needs to be encapsulated. IPv4 is an example of a passenger protocol.
  - Carrier protocol—The protocol that is used to encapsulate the passenger protocol. Cisco NX-OS supports GRE as a carrier protocol.
  - Transport protocol—The protocol that is used to carry the encapsulated protocol. IPv4 is an example of a transport protocol. An IP tunnel takes a passenger protocol, such as IPv4, and encapsulates that protocol within a carrier protocol, such as GRE. The device then transmits this carrier protocol over a transport protocol, such as IPv4.

- You configure a tunnel interface with matching characteristics on each end of the tunnel.
- You must enable the tunneling feature in a device before you can configure and enable any IP tunnels.
- Cisco NX-OS supports the following maximum number of tunnels: IP tunnels - 8 tunnels
- Cisco NX-OS does not support GRE tunnel keepalives.
- The IP tunnel interface cannot be configured to be a span source or destination.
- IP tunnels do not support PIM or other Multicast features and protocols.
- The feature tunnel feature on Cisco Nexus 9000 switches cannot co-exist with the VXLAN feature feature nv overlay.

## Basic Commands
```
feature tunnel
interface tunnel X
tunnel mode gre ip
tunnel source ex/x ---> puede ser una loopback o SVI
tunnel destination x.x.x.x
ip add x.x.x.x/x
no shut
```

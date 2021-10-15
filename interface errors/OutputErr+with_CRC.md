[+] As mentioned before, the output errors on Nexus 9000 interfaces in conjunction with CRC errors on the remote device are usually a byproduct of the default cut-through switching mode of Nexus devices. 

[+] This means that when the Nexus just receives the first part of the ethernet frame that has the destination mac address then it will make the forwarding decision and start sending the first part of the packet out the egress port even before the full packet is received on the ingress port.

[+] It will modify the FCS to be blatantly incorrect (known as a "stomp"), continue forwarding the packet out of its destined egress interface, and incrementing the output error counter on the ingress interface.

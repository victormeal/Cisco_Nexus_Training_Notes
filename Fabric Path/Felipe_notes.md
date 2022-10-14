Path of a Packet Through FabricPath
==========================
+ FP uses 3 Tables to Forward Packets
1. MAC address table - VLAN, MAC Address, Port (local or remote) , FTAG (for non unicast)
2. Switch ID Table (remote Switch ID, local next-hop interface)
3. Multi Destination Tree Table - Per Tree SwitchID information, next-hop interfaces 
 
Known Unicast Packet:
+ For a unicast packet the packet will ingress on a CE edge port and be forwarded based on MAC table learning to a FP device and will map the MAC to a Destination Switch ID (DSWID), once the FP device receives that packet it will add the Switch ID and the destination Switch ID (DSID) to the packet. The DSID comes from the MAC address for the destination MAC information received from the source device on the CE port), the packet will then go through the FP network and be forwarded based on Destination Switch ID (not mac addresses like in a normal CE environment), at each FP hop the TTL will decrease by 1 at each FP switch (to prevent a packet from infinitely looping), until it finally gets to the last switch and egresses the CE edge port out to its destination. 
 
Broadcast, Unknown Unicast, Multicast:
For BUM traffic the path of the packet will be as follows:
Packet is received the same way via a CE edge port with the destination and source MAC addresses and sent to a FP switch. The FP switch then adds the FTAG value as well as the Switch ID and the DSID which will be a Flooded ID packet, and the packet will go through the FP network based on the FTAG/Tree topology until it egresses out of a CE edge port to the destination
 
Important things to remember in path of the packet:
** only FP vlans will be carried over FP interfaces
** FP VLANs can be mixed with CE Vlans on the Edge interfaces
** A VLAN can only be one of two options: CE or FP 
 
 
Best Practice (Read the FP Best Practice Guide Linked Below)
===========
+ Manually configure the FP Switch ID when configuring FP (as well as emulated SWID) 
+ Configure VLANs allowed in vPC+ member ports as mode fabricpath VLANs
+ All VLANs in fabricpath mode should exist on all FP devices
+ Using port channels between spine and leafs
+ Configuring AnyCast HSRP with vPC+ devices
 
 
Useful Commands
==============
show run fabricpath
show fabricpath switch-id <<== this will give you the local switch id , emulated switch ID and all the other switch IDs the fabric path switch knows about
show fabricpath isis interface brief <<== shows you interface in FP mode
show fabricpath isis adjacency
show fabricpath route switch id <number> <<== this will give you the interface that takes you to the next hop in the FP network (this is how you map the path of the packet in FP)
show fabricpath isis topology summary <<== gives you the FTAG numbers (if more than 1) and the tree ID and who is the root Switch ID for the FP Topology

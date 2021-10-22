First of all, MTS stands for Messaging and Transaction Service. Think of it as a Messaging broker for Inter Process communications.
These communications include event notification, synchronization, and message persistency between system services and system components.
MTS facilitates this communication and also permit persistent sync after a process restart.

I am not sure about the platform you took this output. But the commands should be pretty much the same across N7k, N9k and N9k (ACI)
Let's say Process-A in the Sup wants to sync information, via the Message X, with Process-B in the Sup.

Process-A will use MTS to send the Message-X to Process-B.
MTS assigns a SAP ID to each Process and an OPCODE to each Messsage the processes could use.
The location of the process is called Node, like the Supervisor other Node could a Linecard.
When the communication happens, MTS sees that SAP-A from the Sup is sending OPCODE-X to SAP-B.
Process-B gets notified and pull the message. While this happens, the Message is put in a Messaging buffer.

 

The command "show system internal mts buffers details" display the messaging buffer of MTS.

 

2# show system internal mts buffers details
**Fast Sap Buffers are not displayed below**
Node/Sap/queue Age(ms) SrcNode SrcSAP DstNode DstSAP OPC MsgId MsgSize RRToken Offset
sup/284/pers 182030110 0x301 4461 0x301 284 86017 0x2ba2 4596 0x2ba2 0xfab2004
sup/284/pers 1256 0x301 4725 0x301 284 86017 0x345003 4596 0x345003 0xfab0004
sup-1/284/pers 93151415 0x305 4551 0x305 284 86017 0x1959 4596 0x1959 0xfab6004
sup-1/284/pers 2776 0x305 4466 0x305 284 86017 0x17af6 4596 0x17af6 0xfab4004


Node/Sap/queue - Node could be sup, lc, sup-X. Sap defines the source process. Queue the type of messaging queue of the process.
Age(ms) - How long this message has been staying in the buffer, the value is in miliseconds.
SrcNode - Shows the ID for Node.
SrcSAP - SAP ID for the Source Process
DstNode - ID for the Destination Node
DstSAP - SAP ID for the Destination Process
OPC - OPCODE ID, Defines the specific Operation sent between process.

 

We can then take the information to decode the Messages.

 

Node/Sap/queue sup/284/pers From Supervisor SAP 284 in the Persistent queue

 

Age(ms) 182030110 msecs == 50.5639194443999997 Hours. Almost 2 Days in the queue, these messages shouldn't remain too long here.

 

SrcNode 0x301 Node ID for the Sup

 

SrcSAP 4461 With the command 'show system internal mts sup sap 4461 description' we will have the Source Process (STP, ETHPM, VLANMGR, ETC)


DstNode 0x301 Same Node ID, the Process is also in the Supervisor.

 

DstSAP 284 With the command 'show system internal mts sup sap 284 description' we will have the Destination Process

 

OPC 86017 With the command 'show system internal mts opcodes | in 86017' we will display the Operation, like MTS_OPC_VSAN_MGR_NOTIF_CFG, MTS_OPC_OSPF_LDPSYNC_CHANGE, etc

 

In case you see multiple MTS messages stuck for very long time, please open a case with TAC in order to trace the source for this.

From:

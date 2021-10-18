Output discards:
Output discards happen when we have either burst traffic or oversubscription. 
Burst traffic means that there are moments when the traffic trying to be sent through a 10G interface (for example) is more than 10 Gbps, when this happens Nexus will be using buffers but in these cases they are not enough to handle that amount of traffic and Nexus starts discarding packets. We will not see those moments in the “show interface” outputs because “input rate” and “output rate” are average per 30 seconds (Load-Interval #1) and 300 seconds (Load-Interval #2).
Oversubscription is easier to observe, as we can normally see a usage near to 10Gps with “show interface” outputs.
 
The best way to address output discards is by creating port-channels for the affected single interfaces and add more members to the affected port-channels. 


A partir de la 7.0(3)I7(7) y 9.2(4) se puede ver este log cuando se excede el uso de buffer sobre 90%

<timestamp> <hostname> %TAHUSD-SLOT1-4-BUFFER_THRESHOLD_EXCEEDED: Module 1 Instance 0 Pool-group buffer 90 percent threshold is exceeded!

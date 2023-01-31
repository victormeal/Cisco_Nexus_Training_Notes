# Buffer Threshold exceeded syslog

https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/217340-understand-the-tahusd-buffer-threshold-e.html

# Output discards:
Output discards happen when we have either burst traffic or oversubscription. 
Burst traffic means that there are moments when the traffic trying to be sent through a 10G interface (for example) is more than 10 Gbps, when this happens Nexus will be using buffers but in these cases they are not enough to handle that amount of traffic and Nexus starts discarding packets. We will not see those moments in the “show interface” outputs because “input rate” and “output rate” are average per 30 seconds (Load-Interval #1) and 300 seconds (Load-Interval #2).
Oversubscription is easier to observe, as we can normally see a usage near to 10Gps with “show interface” outputs.
 
The best way to address output discards is by creating port-channels for the affected single interfaces and add more members to the affected port-channels. 


A partir de la 7.0(3)I7(7) y 9.2(4) se puede ver este log cuando se excede el uso de buffer sobre 90%

<timestamp> <hostname> %TAHUSD-SLOT1-4-BUFFER_THRESHOLD_EXCEEDED: Module 1 Instance 0 Pool-group buffer 90 percent threshold is exceeded!
 
 =====
 
 Hello team,

Thank you for your time in the Webex today! Let me send you a brief summary of it.
You were troubleshooting a latency issue (milliseconds delay) on your environment and found below log present on Nexus switches. It was not clear to you if this log indicates a drop or not. Hence, you opened this case to investigate further.

2022 Nov 23 17:13:15.904 clf-pfxr1-l1 %TAHUSD-SLOT1-4-BUFFER_THRESHOLD_EXCEEDED: Module 1 Instance 0 Pool-group buffer 90 percent threshold is exceeded!
2022 Nov 23 17:13:20.913 clf-pfxr1-l1 %TAHUSD-SLOT1-4-BUFFER_THRESHOLD_EXCEEDED: Module 1 Instance 1 Pool-group buffer 90 percent threshold is exceeded!

These messages mean that the buffers of the interfaces are above 90% of usage, this can lead to output discards due to congestion. Please notice that this is only an informative log, which warns you to look for output discards.

Output discards happen when we have either burst traffic or oversubscription.
Burst traffic means that there are moments (usually milliseconds) when the traffic trying to be sent through a 10G interface (for example) is more than 10 Gbps, when this happens Nexus will be using buffers but, in these cases, they are not enough to handle that amount of traffic and Nexus starts discarding packets. We will not see those moments in the “show interface” outputs because “input rate” and “output rate” are average per 30 seconds (Load-Interval #1) and 300 seconds (Load-Interval #2).
Oversubscription is easier to observe, as we can normally see a usage near to 10Gps with “show interface” outputs.

Usually, the first option to address output discards is by creating port-channels for the affected single interfaces and add more members to the affected port-channels. However, if you identify that the port is usually exhausted by one expected flow (or a set of expected flows hashed to the same physical member port), then it’s better to upgrade your port. For example: if it’s a 10G port, then let’s use a 25G port.

If you want to understand what traffic is bursty and exhausting your port, then you can configure Netflow on devices connected to these interfaces (as egress netflow is not supported on Nexus 9K). Other option is to configure SPAN captures on the port where output discards are increasing. However, we need to make sure the computer/server where Wireshark will be running is able to handle that much traffic without crashing. Please find below a guide for the SPAN/Wireshark approach, it is for Catalyst switches but it’s the same idea.
https://www.cisco.com/c/en/us/support/docs/lan-switching/switched-port-analyzer-span/116260-technote-wireshark-00.html

Usually, if output discards do not increase that fast and that much, you can safely ignore them. However, in this case, you want to address a latency of milliseconds. So, it looks like we cannot really ignore these counters, even if they are not increasing that much.

As we agreed, I will keep this case opened in monitoring state. Please let me know once the next call is scheduled, that way I can confirm my availability and join if possible. Kindly let me know if you have any question.


Best regards.

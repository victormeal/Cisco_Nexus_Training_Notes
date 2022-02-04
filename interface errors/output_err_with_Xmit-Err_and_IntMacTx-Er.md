The N9K-C93180YC-EX switch is a cut-through switch, meaning it starts forwarding a frame before it is fully received. If any corruption is detected during the forwarding of this frame, the switch cannot interrupt the forwarding process anymore. Instead, what the switch does is to change the frame's CRC to a special value called the stomped CRC. This special value indicates to the next receiver that the frame was already corrupt when it arrived to the switch in the first place, and did not get corrupted on the link toward that receiver.

There are multiple reasons why the N9K can declare the frame as corrupt, and stomp its CRC, and some of them are:
1.	The frame's initial CRC already did not match the computed CRC (meaning the frame was corrupt during arrival)
2.	The frame's initial CRC was already stomped (meaning that the corruption occurred somewhere upstream, and the immediately preceding switch is also a cut-through switch performing the stomping)
3.	The frame's size exceeds the ingress interface's MTU
These errored frames reported as Rx frames with stomped CRC are the ones that are causing the output errors shown on the N9K switch.

The stomped errors did not increase during your data collection. This might be because the Nexus might be receiving already corrupted packets for other device on the network, but we don’t know the ingress interface yet. The egress interface is eth1/35 where we are seeing the output erros with Xmit-Err and IntMacTx-Er.

Up to this moment what we can do is to change the switching mode of the nexus to store and forward, with that change we will be able to see interfaces incrementing their count for CRC hence we will identify the source of the corrupted packets. To chance the switching mode, apply the following command:
n9K# conf t
n9K(config)# switching-mode store-forward
If you want to go back to cut-through switching apply the following command:
n9K# conf t
n9K(config)# no switching-mode store-forward

After changing the switching-mode to store-forward, clear the counters and collect the output of `show interface` and `show interface counter errors`

You can read more about the switching modes on the following documentation:
Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide, Release 7.x - Chapter: Configuring Switching Modes
Store-and-Forward Switching Mode
When store-and-forward switching is enabled, the switch checks each frame for cyclic redundancy check (CRC) errors before forwarding them to the network. Each frame is stored until the entire frame has been received and checked.
Because it waits to forward the frame until the entire frame has been received and checked, the switching speed in store-and-forward switching mode is slower than the switching speed in cut-through switching mode.





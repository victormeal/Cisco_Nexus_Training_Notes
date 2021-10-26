After looking into the outputs provided and researching the drop reasons that are given by this switch, I can say that this traffic is being dropped due to either incorrect VLAN headers or there possibly could be an issue with the ASIC that controls this port.

I am partially assuming that you are not seeing drops along other ports, is this correct? If you are not, this can basically remove the ASIC failure.

This would leave the incorrect/corrupted VLAN headers. The drops sited in the 'sh hardware internal interface asic counters module 1' output under  'DROP_SRC_VLAN_MBR' indicate a unexpected VLAN header on ingress traffic to port Eth1/3. The 'DROP_UC_DF_CHECK_FAILURE' errors are a byproduct of loop avoidance, and can be ignored. Here are the snippets from the outputs:

MN111-9K2-SW# sh hardware internal interface asic counters module 1
Important Counters/Drops
--------------- ------------------------------------------------------------------------------------------------
Interface Name  Drop Reasons for the Interface, See below output for detail if any
--------------- ------------------------------------------------------------------------------------------------
                |9|9|9|9|9|9|8|8|8|8|8|8|8|8|8|8|7|7|7|7|7|7|7|7|7|7|6|6|6|6|6|6|6|6|6|6|5|5|5|5|5|5|5|5|5|5|4|4|4|4|4|4|3|2|2|2|2|2|1|1|1|1|1|1|1|1|1|0|0|0|0|
0|0|0|0
                |5|4|3|2|1|0|9|8|7|6|5|4|3|2|1|0|9|8|7|6|5|4|3|2|1|0|9|8|7|6|5|4|3|2|1|0|9|8|7|6|5|4|3|2|1|0|6|5|4|3|1|0|8|6|5|2|1|0|9|8|6|5|4|3|2|1|0|9|8|7|6|
5|4|3|1
...
Ethernet1/3     |.|.|.|.|.|.|.|.|.|.|.|X|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|X|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|.|
.|.|.|.
...
41 : TAHOE Ingress DROP_VLAN_XLATE_MISS
51 : TAHOE Ingress DROP_SRC_VLAN_MBR
67 : TAHOE Ingress DROP_ACL_DROP
84 : TAHOE Ingress DROP_UC_DF_CHECK_FAILURE

I would like to either schedule a Webex to get a capture of traffic on the peer link. Let me know when you may be available, or if this is not an option, as I can prepare the commands need for the capture.
I also would like to know if you are seeing similar drops along any of the links coming from the 7K pair or on it's peer-link.

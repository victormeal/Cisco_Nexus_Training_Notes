# MAC Authentication Bypass (MAB)

When you enable MAB on a switchport, the switch drops all frames except for the first frame to learn the MAC address. Pretty much any frame can be used to learn the MAC address except for CDP, LLDP, STP, and DTP traffic. Once the switch has learned the MAC address, it contacts an authentication server (RADIUS) to check if it permits the MAC address.

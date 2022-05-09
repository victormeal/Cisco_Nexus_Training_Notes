# MAC Authentication Bypass (MAB)

When you enable MAB on a switchport, the switch drops all frames except for the first frame to learn the MAC address. Pretty much any frame can be used to learn the MAC address except for CDP, LLDP, STP, and DTP traffic. Once the switch has learned the MAC address, it contacts an authentication server (RADIUS) to check if it permits the MAC address.

By default, MAB only supports a single endpoint (device) per switchport. When it sees more than one source MAC address, it causes a security violation. This can be an issue when for example, you use an IP phone with a PC behind it. It’s possible to change this behavior:

Single-host mode: only a single source MAC address can be authenticated. When the switch detects another source MAC address after authentication, it triggers a security violation. This is the default setting.
Multi-domain authentication host mode: you can authenticate two source MAC addresses, one in the voice VLAN and another one in the data VLAN. This is for the scenario where you have an IP phone and a PC on a single switchport. Any more source MAC addresses trigger a security violation.
Multi-authentication host mode: you can authenticate multiple source MAC addresses. You can use this when your switchport is connected to another switch. Each source MAC address is separately authenticated.
Multi-host mode: the switch allows multiple source MAC addresses. Only the first source MAC address is authenticated, all other source MAC addresses are automatically permitted.

https://networklessons.com/cisco/ccie-routing-switching-written/mac-authentication-bypass-mab

----


# SPAN

## SPAN Sources
	- Ethernet ports
	- A port configured as a source port cannot also be configured as a destination port

## Span Destinations
	- Ethernet ports in either access or trunk mode
	- A port configured as a destination port cannot also be configured as a source port.
	- A destination port can be configured in only one SPAN session at a time.
	- Destination ports do not participate in any spanning tree instance. SPAN output includes bridge protocol data unit (BPDU) Spanning Tree Protocol hello packets.

## Span Sessions

## Guidelines and Limitations for SPAN
	- All SPAN replication is performed in the hardware. The supervisor CPU is not involved.
	- You can configure only one destination port in a SPAN session.
	- Configuring two SPAN or ERSPAN sessions on the same source interface with only one filter is not supported. If the same source is used in multiple SPAN or ERSPAN sessions either all the sessions must have different filters or no sessions should have filters.
	- SPAN is not supported for management ports.
	- the same source can be part of multiple sessions.

# STP Enhancements

## Root Guard

The configuration of root guard is on a per-port basis. Root guard does not allow the port to become an STP root port, so the port is always STP-designated. If a better BPDU arrives on this port, root guard does not take the BPDU into account and elect a new STP root. Instead, root guard puts the port into the root-inconsistent STP state. You must enable root guard on all ports where the root bridge should not appear. In a way, you can configure a perimeter around the part of the network where the STP root is able to be located.
Root guard puts the port in the root-inconsistent STP state. No traffic passes through the port in this state.After device D ceases to send superior BPDUs, the port is unblocked again. Via STP, the port goes from the listening state to the learning state, and eventually transitions to the forwarding state. Recovery is automatic; no human intervention is necessary.

- What Is the Difference Between STP BPDU Guard and STP Root Guard?
BPDU guard and root guard are similar, but their impact is different. BPDU guard disables the port upon BPDU reception if PortFast is enabled on the port. The disablement effectively denies devices behind such ports from participation in STP. You must manually reenable the port that is put into errdisable state or configure errdisable-timeout.

Root guard allows the device to participate in STP as long as the device does not try to become the root. If root guard blocks the port, subsequent recovery is automatic. Recovery occurs as soon as the offending device ceases to send superior BPDUs.

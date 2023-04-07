# ETHPM_events.md

[+] ETH_PORT_FSM_EV_SFP_NOT_PRESENT – It indicates the SFP removal and absence.
[+] ETH_PORT_FSM_EV_FCOT_INSERTED – It represents SFP insertion
[+] ETH_PORT_FSM_EV_SFP_VALID – It represents SFP validation on Nexus.

[+] ETH_PORT_FSM_EV_PACER_TIMER_EXPIRED – The pacer timer is the amount of time ethpm waits for heartbeat to be received to bring up the port. Hence we did not receive the same within the required time thus pacer timer expired. This usually points to a L1 issue with bad cable/SFP or a bad port on the switch. It can also be due to MTS buffer stuck.

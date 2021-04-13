# Cisco Nexus 9000 Series NX-OS Software Upgrade and Downgrade Guide, Release 7.x

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/upgrade/guide/b_Cisco_Nexus_9000_Series_NX-OS_Software_Upgrade_and_Downgrade_Guide_Release_7x/b_Cisco_Nexus_9000_Series_NX-OS_Software_Upgrade_and_Downgrade_Guide_Release_7x_chapter_010.html

## NX OS Image

- The image filename begins with "nxos" [beginning with Cisco NX-OS Release 7.0(3)I2(1)] or "n9000" (for example, nxos.7.0.3.I2.1.bin or n9000-dk9.7.0.3.I1.1.bin)

## ISSU (in-service software upgrade)

- An ISSU allows you to upgrade the device software while the switch continues to forward traffic. ISSU reduces or eliminates the downtime typically caused by software upgrades.
- The default upgrade process is disruptive. Therefore, ISSU needs to be enabled using the command-line interface (CLI).
- The guest shell is disabled during the ISSU process and it is later reactivated after the upgrade.

### Performing Standard ISSU on Top-of-Rack (ToR) Switches with a Single Supervisor

- The data plane traffic is not disrupted during the ISSU process.
- The control plane downtime during the ISSU process is approximately less than 120 seconds.

### Performing Standard ISSU on End-of-Row (EoR) Switches with Two Supervisors

- Cisco Nexus 9500 Series switches are the modular EoR switches that require two supervisors for ISSU.
- The minimum configuration required is two system controllers and two fabric modules.
- Cisco Nexus 9500 Series switches support parallel upgrade as the default method.

#### Cisco Nexus 9500 Parallel Upgrade Process:
1. First half of line cards.
2. First half of fabric modules.
3. Second half of line cards.
4. Second half of fabric modules.
5. First system controller.
6. Second system controller.

- While performing standard ISSU for the modular switches, the data plane traffic is not disrupted. 
- The control plane downtime is approximately less than 6 Seconds.

### Performing Enhanced ISSU on Top-of-Rack (ToR) Switches with a Single Supervisor

- Configuring the enhanced or container based ISSU on single supervisor ToRs is accomplished by creating virtual instances of the supervisor modules and the line cards.
- The supervisor is upgraded first and the line card is upgraded next.
- Enhanced ISSU requires 16G memory on the switch.

```
switch(config)# boot mode lxc
Using LXC boot mode
Please save the configuration and reload system to switch into the LXC mode.
switch(config)# copy r s
[########################################] 100%
Copy complete.
```
- The data plane traffic is not disrupted during the ISSU process.
- The control plane downtime is less than 6 seconds.

## Useful Commands

- Verify that you have no active configuration sessions:
´Show configuration session summary´
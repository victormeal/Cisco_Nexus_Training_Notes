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
`Show configuration session summary`

- Ping file server:
`switch# ping 192.0.2.100 vrf management`

- Verify compatibility:
`show incompatibility nxos bootflash:filename`

- Version
`switch#  show version`

## Upgrade Patch Instructions

On Cisco Nexus 9500 series switches only, a software upgrade from Cisco NX-OS Release 7.0(3)I1(2), 7.0(3)I1(3), or 7.0(3)I1(3a) to any other Cisco NX-OS release requires installing two patches prior to upgrading using the install all command.

1. Add both patches with the **install add bootflash:{patch-file.bin}** command.
```
switch(config)# install add bootflash:n9000-dk9.7.0.3.I1.2.CSCuy16604.bin
Install operation 16 completed successfully at Thu Mar  3 04:24:13 2016
switch(config)# install add bootflash:n9000-dk9.7.0.3.I1.2.CSCuy16606.bin
Install operation 17 completed successfully at Thu Mar  3 04:24:43 2016
```

2. Activate both patches with the **install activate {patch-file.bin}** command.
```
switch(config)# install activate n9000-dk9.7.0.3.I1.2.CSCuy16604.bin
Install operation 18 completed successfully at Thu Mar  3 04:28:38 2016
switch (config)# install activate n9000-dk9.7.0.3.I1.2.CSCuy16606.bin
Install operation 19 completed successfully at Thu Mar  3 04:29:08 2016
```

3. Commit both patches with the **install commit {patch-file.bin}** command.
```
switch(config)# install commit n9000-dk9.7.0.3.I1.2.CSCuy16604.bin
Install operation 20 completed successfully at Thu Mar  3 04:30:38 2016
switch (config)# install commit n9000-dk9.7.0.3.I1.2.CSCuy16606.bin
Install operation 21 completed successfully at Thu Mar  3 04:31:16 2016
```

4. Proceed with an NX-OS software upgrade to the desired target release with the **install all** command.
```
switch (config)# install all nxos bootflash:nxos.7.0.3.I7.1.bin
Installer will perform compatibility check first. Please wait.
uri is: /nxos.7.0.3.I7.1.bin
Installer is forced disruptive

Verifying image bootflash:/nxos.7.0.3.I7.1.bin for boot variable "nxos".
[####################] 100% -- SUCCESS

-- output omitted --
```

## Configuring Enhanced ISSU

Beginning with Cisco NX-OS Release 7.0(3)I5(1), you can enable or disable enhanced (LXC) ISSU.

1. Enters global configuration mode. **configure terminal**
```
switch# configure terminal
switch(config#)
```

2. Enables or disables enhanced (LXC) ISSU. **[no] boot mode lxc**
```
switch(config)# boot mode lxc
Using LXC boot mode
```

3. (Optional) Shows whether enhanced (LXC) ISSU is enabled or disabled. **show boot mode**
```
switch(config)# show boot mode
LXC boot mode is enabled
```

4. Saves the running configuration to the startup configuration. **copy running-config startup-config**
`switch(config)# copy running-config startup-config`

5. Reloads the device. When prompted, press Y to confirm the reboot. **reload**
```
switch(config)# reload
This command will reboot the system. (y/n)?  [n] Y
loader>
```

## Upgrading the Cisco NX-OS Software

Use this procedure to upgrade a Cisco Nexus 9000 Series switch to a Cisco NX-OS 7.x release.

1. Read realese notes.

2. Log in to the device on the console port connection.

3. Ensure that the required space is available for the image file to be copied.
```
switch# dir bootflash:
49152    Dec 10 14:43:39 2015 lost+found/
80850712 Dec 10 15:57:44 2015 n9000-dk9.7.0.3.I1.1.bin
...

Usage for bootflash://sup-local
 4825743360 bytes used
16312102912 bytes free
21137846272 bytes total
```

4. If you need more space on the active supervisor module, delete unnecessary files to make space available.
`switch# delete bootflash:n9000-dk9.7.0.3.I1.1.bin`

5. Verify that there is space available on the standby supervisor module.
```
switch# dir bootflash://sup-standby/
49152    Dec 10 14:43:39 2015 lost+found/
80850712 Dec 10 15:57:44 2015 n9000-dk9.7.0.3.I1.1.bin
...

Usage for bootflash://sup-standby
 4825743360 bytes used
16312102912 bytes free
21137846272 bytes total
```

6. If you need more space on the standby supervisor module, delete any unnecessary files to make space available.
`switch# delete bootflash://sup-standby/n9000-dk9.7.0.3.I1.1.bin`

7. Log in to Cisco.com, choose the software image file for your device from the following URL, and download it to a file server: http://software.cisco.com/download/navigator.html.

8. Copy the software image to the active supervisor module using a transfer protocol. You can use FTP, TFTP, SCP, or SFTP.
`switch# copy scp://user@scpserver.cisco.com//download/nxos.7.0.3.I2.1.bin bootflash:nxos.7.0.3.I2.1.bin`

9. Display the SHA256 checksum for the file to verify the operating system integrity and ensure that the downloaded image is safe to install and use.
```
switch# show file bootflash://sup-1/nxos.7.0.3.I2.1.bin sha256sum
5214d563b7985ddad67d52658af573d6c64e5a9792b35c458f5296f954bc53be
```

10. Check the impact of upgrading the software before actually performing the upgrade.
```
switch# show install all impact nxos bootflash:nxos.7.0.3.I2.1.bin
Installer will perform compatibility check first. Please wait. 
uri is: /nxos.7.0.3.I2.1.bin
Installer is forced disruptive 

Verifying image bootflash:/nxos.7.0.3.I2.1.bin for boot variable "nxos".
[####################] 100% -- SUCCESS
-- Output omitted --
```

11. Save the running configuration to the startup configuration.
`switch# copy running-config startup-config`

12. Upgrade the Cisco NX-OS software using the **install all nxos bootflash:filename [no-reload | non-disruptive | non-interruptive | serial]** command.
`switch# install all nxos bootflash:nxos.7.0.3.I2.1.bin``

    - no-reload—Exits the software upgrade process before the device is reloaded.

    - non-disruptive—Performs an in-service software upgrade (ISSU) to prevent the disruption of data traffic. (By default, the software upgrade process is disruptive.)

    - non-interruptive—Upgrades the software without any prompts. This option skips all error and sanity checks.

    - serial—Upgrades the I/O modules in Cisco Nexus 9500 Series switches one at a time. (By default, the I/O modules are upgraded in parallel, which reduces the overall upgrade time. Specifically, the I/O modules are upgraded in parallel in this order: the first half of the line cards and fabric modules, the second half of the line cards and fabric modules, the first system controller, the second system controller.)

13. (Optional) Display the entire upgrade process.
`switch# show install all status`

14. (Optional) Log in and verify that the device is running the required software version.
`switch# show version`

15. (Optional) If necessary, install any licenses to ensure that the required features are available on the device.

## Upgrading the Cisco NX-OS Software for Cisco Nexus 9500 Platform Switches with -R Line Cards
Use this procedure to upgrade a Cisco Nexus 9500 platform switch with an -R line card to a Cisco NX-OS 7.x release.
...

## Upgrade Process for vPCs
### Upgrade Process for a vPC Topology on the Primary Switch
The following list summarizes the upgrade process on a switch in a vPC topology that holds either the Primary or Operational Primary vPC roles. Steps that differ from a switch upgrade in a non-vPC topology are in bold.
...

### Upgrade Process for a vPC Topology on the Secondary Switch
The following list summarizes the upgrade process on a switch in a vPC topology that holds either the Secondary or Operational Secondary vPC roles. Steps that differ from a switch upgrade in a non-vPC topology are in bold.
...

## Downgrading to an Earlier Software Release
Use this procedure to downgrade a Cisco Nexus 9000 Series switch to a Cisco NX-OS 7.x release.

1. Read the release notes

2. Log in to the device on the console port connection.

3. Verify that the image file for the downgrade is present on the active supervisor module bootflash:.
```
switch# dir bootflash:
49152 Aug 01 14:43:39 2015 lost+found/
80850712 Aug 01 15:57:44 2015 nxos.7.0.3.I2.1.bin
...

Usage for bootflash://sup-local
 4825743360 bytes used
16312102912 bytes free
21137846272 bytes total
```

4. If the software image file is not present, log in to Cisco.com, choose the software image file for your device from the following URL, and download it to a file server: http://software.cisco.com/download/navigator.html.

5. Copy the software image to the active supervisor module using a transfer protocol. You can use FTP, TFTP, SCP, or SFTP.
`switch# copy scp://user@scpserver.cisco.com//download/n9000-dk9.7.0.3.I1.1.bin bootflash:n9000-dk9.7.0.3.I1.1.bin`

6. Check for any software incompatibilities.
```
switch# show incompatibility-all nxos bootflash:n9000-dk9.7.0.3.I1.1.bin
Checking incompatible configuration(s) 
No incompatible configurations
```

7. Disable any features that are incompatible with the downgrade image.

8. Check for any hardware incompatibilities.
```
switch# show install all impact nxos bootflash:n9000-dk9.7.0.3.I1.1.bin
Installer will perform compatibility check first. Please wait. 
uri is: /n9000-dk9.7.0.3.I1.1.bin
Installer is forced disruptive

Verifying image bootflash:/n9000-dk9.7.0.3.I1.1.bin for boot variable "nxos".
[####################] 100% -- SUCCESS
--- Output omitted ---
```

9. Power off any unsupported modules.
`switch# poweroff module module-number`

10. Save the running configuration to the startup configuration.
`switch# copy running-config startup-config`

11. Downgrade the Cisco NX-OS software.
`switch# install all nxos bootflash:n9000-dk9.7.0.3.I1.1.bin`

12. (Optional) Display the entire downgrade process
`switch# show install all status`

13. (Optional) Log in and verify that the device is running the required software version.
`switch# show version`

## Downgrading the Cisco NX-OS Software for Cisco Nexus 9500 Platform Switches with -R Line Cards
...

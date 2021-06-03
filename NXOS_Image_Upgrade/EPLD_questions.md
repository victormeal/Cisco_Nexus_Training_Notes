Regarding the questions about ACI I cant give you a detailed answer because in our team we dont provide support to ACI platforms, we only cover NX-OS platforms. But feel free to open a case with the ACI team to have a better response.

1. Do I need to upgrade the EPLD image equitant to this IOS image?
 Cisco provides the latest EPLD images with each release. Typically, these images are the same as provided in earlier releases but occasionally some of these images are updated. These EPLD image updates are not mandatory unless otherwise specified.
 When new EPLD images are available, the upgrades are always recommended if your network environment allows for a maintenance period in which some level of traffic disruption is acceptable. If such a disruption is not acceptable, then consider postponing the upgrade until a better time.

2. How can I check the current running Epld image ?
 You can check the current EPLD image with the command `show version module <module number> epld`

3. How can I verify the current Epld image is compatible to nxos.9.3.7.bin?
 To determine which devices need upgraded EPLDs, use the `show install impact epld <epld image>` command for a device and indicate the latest EPLD image file. The output for this command indicates the current EPLD images, new EPLD images, and whether the upgrades would be disruptive to switch operations. If the currently installed EPLD version number is greater than the new EPLD image number, you can skip the upgrade.

 Also, I share with you the next documentation:

- Cisco Nexus 9000 Series FPGA/EPLD Upgrade Release Notes, Release 7.0(3)I7(7)
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/epld_rn/guide/nxos_n9K_epldRN_703i77.html

Recommended but not mandatory EPLD images for the release 7.0(3)I7(7):
 -	IOFPGA: 0x10
 -	MIFPGA1: 0x8
 -	MIFPGA2: 0x12

- Cisco Nexus 9000 Series FPGA/EPLD Upgrade Release Notes, Release 9.3(7)
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/epld-rn/nxos_n9K_epldRN_937.html

Recommended but not mandatory EPLD images for the release 9.3(7):
 -	IOFPGA: 0x13
 -	MIFPGA1: 0x12
 -	MIFPGA2: 0x8

I hope you find this information useful.


# Basic NX OS Upgrade
Recommended Cisco NX-OS Releases for Cisco Nexus 9000 Series Switches

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/recommended_release/b_Minimum_and_Recommended_Cisco_NX-OS_Releases_for_Cisco_Nexus_9000_Series_Switches.html

## Check upgrade path
https://www.cisco.com/c/dam/en/us/td/docs/Website/datacenter/ISSUmatrix/index.html

## Get image
https://software.cisco.com/download/home

### from USB
- `dir` -> por default bootflash
```
dir usb1
copy usb1:nxos.9.3.5.bin bootflash:
```

### from stfp server
```
feature stfp-server
feature scp-server
copy sftp:nxos.9.3.5bin bootflash:
	management
	172.18.121.75
	admin
```

### form PC (filezilla)
- host:10.106.47.113
- username:admin
- password:*******
- drag files form pc to nexus (bootflash)


### check integrity of image, checksum
Must match with documentation:
`sh file bootflash:nxos.9.3.5.bin md5sum`


## Upgrade

### check impact
`show install all impact nxos bootflash:nxos.9.3.5.bin`

### call upgrade (disruptive)
`install all nxos bootflash:nxos.9.3.5.bin`

### ISSU (non-disruptive)
`install all nxos bootflash:nxos.9.3.5.bin non-disruptive`





IP Address : 10.76.76.160
username: tftpuser
password: C!sco123


RTP
172.18.121.75/

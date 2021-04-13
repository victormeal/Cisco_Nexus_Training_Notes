# Basic NX OS Upgrade

## obtener la imagen
https://software.cisco.com/download/home

### desde un usb
- ´dir´ -> por default bootflash
´´´
dir usb1
copy usb1:nxos.9.3.5.bin bootflash:
´´´

### desde el stfp server
´´´
feature stfp-server
feature scp-server
copy sftp:nxos.9.3.5bin bootflash:
	management
	172.18.121.75
	admin
´´´

### desde la compu (filezilla)
- host:10.106.47.113
- username:admin
- password:*******
- arrastras los archivos de la compu al nexus (bootflash)


### revisar integridad, checksum
Revisar que coincida con lo documentado:
´sh file bootflash:nxos.9.3.5.bin md5sum´


## Actualizar

### call upgrade (disruptive)
´install all nxos bootflash:nxos.9.3.5.bin´

### ISSU (non-disruptive)
´install all nxos bootflash:nxos.9.3.5.bin non-disruptive´





IP Address : 10.76.76.160
username: tftpuser
password: C!sco123


RTP
172.18.121.75/

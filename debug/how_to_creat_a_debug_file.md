# how_to_creat_a_debug_file
```
Nexus# debug logfile TEST101
Nexus# debug ip ospf all
Nexus# undebug all
Nexus# show debug logfile TEST101
You can also send logs to a server using ftp, tftp, sctp, et: ( there is no 
equivalent command on NX-OS that matches the 'Logging trap debug' to send debug 
messages to a server automatically. The closest match to this functionality is 
to create a file where debugs are stored, and then send those to the server 
manually)

Nexus# show debug logfile TEST101 > ?
              bootflash: Destination filesystem path
              ftp: Destination filesystem path
              scp: Destination filesystem path
              sftp: Destination filesystem path
              slot0: Destination filesystem path
              tftp: Destination filesystem path
              usb1: Destination filesystem path
              usb2: Destination filesystem path
              volatile: Destination filesystem path
```

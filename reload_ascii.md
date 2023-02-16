# Reload ascii

1.  Advised that the logs/outputs did not show a reason for the config loss.

You asked for more info on the 'reload ascii' command suggested by the previous Engineer;-

```

The normal 'reload' command performs a 'Binary Reload'. The main difference between "Binary Reload" and "Reload ASCII" is the way a switch uses to apply its start-up configuration:



1. With a "Binary Reload", the switch applies its start-up configuration using the saved binary file which is the configuration saved after "copy run start"

2. With a "Reload ASCII", the switch will delete the binary file of the start-up configuration, and re-build the start-up configuration from the ASCII file saved in NVRAM, which is one more step than a "Binary Reload". After the start-up configuration is built, the remaining process will be same as "Binary Reload". The 'reload ascii'

```

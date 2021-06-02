# Password recovery N9K

Next, I will describe the process.

- Step 0: You need to be connected to the console port.
- Step 1: Power cycle the Nexus.
- Step 2: When the Nexus is starting up press Ctrl-C, I suggest you start pressing these keys as soon as possible to make sure you get it on the first try.
- Step 3: The prompt should have changed to `loader>`
- Step 4: Enter the following command: `cmdline recoverymode = 1`
- Step 5: With the command `dir` you can see which files are the Nexus bootflash. Identify which image file you want to start the nexus from.
- Step 6: For example, if your image file is called “nxos.9.3.7.bin”. With the command `boot nxos.9.3.7.bin` the Nexus will boot with that image.
- Step 7: When the Nexus has finished booting, the prompt will change to `switch (boot)`. Enter configuration mode. `config terminal`
- Step 8: Change the admin password with the command `admin-password YourNewPassword`
- Step 9: Exit the configuration mode and with the` load-nxos` command the nexus will reload.
- Step 10: Now you can try to access the Nexus with the admin user and with the new password.

These steps can also be found at this link:

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/troubleshooting/guide/b-cisco-nexus-9000-nx-os-troubleshooting-guide-93x/b-cisco-nexus-9000-nx-os-troubleshooting-guide-93x_chapter_011.html

I will answer your questions below.


-  What would be the pre-checks before upgrade?
Once you have the image file in the bootflash of your nexus you can check if is not corrupt by checking the MD5 Checksum or/and the SHA512 Checksum. This can be done with the command `sh file bootflash:nxos.9.3.7.bin md5sum ` or `sh file bootflash:nxos.9.3.7.bin sha512`, the output of this commands must match the once that are reported in this site:
https://software.cisco.com/download/home/286195223/type/282088129/release/9.3(7)


  *   For the MD5 Checksum the output must be: 80ba20b23b8695c3f894686b4053c5f6
  *   And for the SHA512 Checksum the output must be: 6b4e12553165a934988c8da42f02503c3ad72ce366ed23002232d013cddcdd3ffed54264f2e18cce349cf2f0e74a346d54a0903bf6e404453af06f9838498e03


You can check if the new NX-OS image is compatible with your Nexus and current configuration with the command `show incompatibility-all nxos bootflash:nxos.9.3.7.bin` . Sometimes there is configuration that needs to be changed to be compatible. The output of this command should tell you which config is not compatible.
You can check which impact the upgrade is going to have with the command `show install all impact nxos bootflash:nxos.9.3.7.bin`, here is going to tell you that the upgrade is going to be disruptive (a reload is needed after the upgrade).

- Any checkpoint, roll-back plan I need to create?
An upgrade should not cause any problem, it's a procedure that take between 5-10 minutes, but be sure to have the configuration backed up. And a maintenance window is suggested because the process is disruptive in this time.


- Do I need to install kick-start image as well?
In old versions the kickstart image and a system image were needed. But in newer ones, like 9.3.7, it`s needed only one file.


- Does this model support ISSU process.
Most of the Nexus support the ISSU process, but it depends on the upgrade path. In this case where we are upgrading from 7.0(3)I7(7) ---> 9.3(7) the ISSU process is not going to be possible. You can try to check it with the command ` sh install all impact nxos bootflash:nxos.9.3.7.bin non-disruptive ` and in the output is going to tell you that it needs to be disruptive.


- Image should be according to SUP module or its common for this platform?
It`s the same image for all the Nexus that supports this image version.


- Procedure to upgrade the image on this platform from current version to 9.3(7).

Step 0. You need to have the image in the Nexus`s bootflash.

(optional) Step1. Check the Checksum of the image and compare it with documentation.
`sh file bootflash:nxos.9.3.7.bin md5sum`
`sh file bootflash:nxos.9.3.7.bin sha512`

(optional) Step2.  With the command ` show incompatibility-all nxos bootflash:nxos.9.3.7.bin`  check if there is no incompatibility.

(optional) Step3. With the command `show install all impact nxos bootflash:nxos.9.3.7.bin` check which impact the upgrade is going to have, it is going to be disruptive.

Step4. When everything is okay proceed with the upgrade with the command ` install all nxos bootflash:nxos.9.3.7.bin`, this command is going to check the same as the command in Step3 and then is going to ask you:

```
Switch will be reloaded for disruptive upgrade.
Do you want to continue with the installation (y/n)?  [n]
```
By typing "y" the upgrade is going to start and is going to take between 5-10 minutes to finish.


- ISSU process
ISSU is not going to be possible to upgrade from 7.0(3)I7(7) ---> 9.3(7). But you can try it with the commands ` sh install all impact nxos bootflash:nxos.9.3.7.bin non-disruptive ` to check impact and if its possible you can perform the upgrade with the command ` install all nxos bootflash:nxos.9.3.7.bin non-disruptive `.


That would be all, I hope I have answered your questions. If you have any comments write me back.
Regards,

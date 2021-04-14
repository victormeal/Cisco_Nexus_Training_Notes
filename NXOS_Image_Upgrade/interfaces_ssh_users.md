# REVIEW - Interface Numbering, SSH Keys, User-Accounts
## Important Points of “Interface Numbering, SSH Keys, User-accounts”

1. Every Interface in Nexus will be represented with keyword “Eth” i.e. Ethernet. It does not matter whether it is Fast Ethernet or Gig Ethernet or Ten Gig Ethernet.
2. In Nexus Interface Numbering starts from 1 i.e. not from 0 ( In IOS or non-Nexus devices Interface Numbering starts from 0”. So do not confuse yourself when someone says “Shutdown the fifth port of second chassis”.  Port will be Eth2/5  not Eth1/4 ( like in IOS)
3. To check Interface capability , use the command “Show Interface INT-NUMBER” e.g “ Show interface Eth3/7” . You can check bandwidth and other parameters using “Show interface status” command as well.
4. Nexus Devices have TWO VRF by default . These are “default” and “management” . We work on “ vrf default” by default. We can create our own VRFs as well.
5. “Telnet Service” is disabled by default on all Nexus devices e.g N7K or N5K. You can verify it using “Show Telnet Server”
6. We can enable “Telnet Service” using “feature telnet”. It means we have to enable the feature of Telnet in configuration mode.
7. “SSH Service” is enabled by default on all nexus devices. It’s version is 2 . You can verify it using “Show ssh service”
    - 1024 Bit RSA key is generated in Nexus devices by default. Verify it using “ Show ssh key”
8. If you want to change the RSA key then use the command “ ssh key RSA 2048 force” . Point To Note is that you will get error message once it deletes the old 1024 key. After that it will ask you to disable feature of SSH . Make sure that you are not logged in using SSH while disabling the feature of SSH. You will end up losing the connection. CARE MUST BE TAKEN.
9. Use the command “ No feature ssh” . After that change the RSA Key using the command ““ ssh key RSA 2048 force” .    “force” keyword is used to forcefully delete the existing key. Then enable the SSH Feature using “ feature ssh”
10. Unlike IOS- we have to create “username and password” to be able to login in Nexus. It asks you to create username password when you initialize it for the first time.
11. To check users, use the command “ Show user-account”. It gives the detail of users , their expiry date and Role.
12. To create users use the command “ username USER-NAME password PASSWORD Role ROLE-NAME” e.g username Ashish Password Ashish Role vdc-operator
13. Username are CASE-SENSITIVE  --( Very Important Observation)
14. The passwords we set while creating user accounts – get converted in to encrypted form – unlike IOS.
15. SSH-key can also be created for every user( Optional) using “ username USER keypair generate rsa 1024 force”  . “Force” keyword is only required when ssh key already exists

## Commands we learnt in this Chapter

- `Show Interface E3/7`
- `Show Interface Status`
- `Show telnet server`
- `Show ssh service`
- `Show ssh key`
- `ssh key RSA 2048 force  ( In config mode)`
- `no feature ssh`
- `ssh key RSA 2048 force `
- `feature ssh`
- `Show user-account`
- `username Ashish Password Ashish Role vdc-operator ( In config mode)`
- `username USER keypair generate rsa 1024 force  ( In config mode)`
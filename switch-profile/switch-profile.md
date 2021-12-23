# Switch-profile
+ This lab was done using two N3k running 9.3.5

## N3K-1
```
N3K-1(config)# cfs ipv4 distribute 
N3K-1(config)# config sync
N3K-1(config-sync)# switch-profile abc
Resyncing db before starting Switch-profile.Re-synchronization of switch-profile db takes a few minutes...
Re-synchronize switch-profile db completed successfully.
Switch-Profile started, Profile ID is 1
N3K-1(config-sync-sp)# sync-peers destination 10.88.171.24         <<< this IP need to be the mgmt0 IP of the peer
```
```
N3K-1(config-sync-sp)# show switch-profile status 

switch-profile  : abc
----------------------------------------------------------

Start-time:  44150 usecs after Thu Dec 23 18:03:43 2021
End-time: 792404 usecs after Thu Dec 23 18:03:44 2021

Profile-Revision: 1
Session-type: Initial-Exchange
Session-subtype: Init-Exchange-All
Peer-triggered: No
Profile-status: Sync Success

Local information:
----------------
Status: Commit Success
Error(s): 

Peer information:
----------------
IP-address: 10.88.171.24
Sync-status: In sync
Status: Commit Success
Error(s): 

N3K-1(config-sync-sp)# 
```

## N3K-2
+ Repeat the same in this device

## Example of configuring a port-channel

+ This can be done in either side.

```
N3K-1(config-sync)# switch-profile abc
N3K-1(config-sync-sp)# int po 99
N3K-1(config-sync-sp-if)# switchport
N3K-1(config-sync-sp-if)# commit
Verification successful...
Proceeding to apply configuration. This might take a while depending on amount of configuration in buffer.
Please avoid other configuration changes during this time.
Commit Successful
```
+ Now the config should be on the other side

## Useful commands
```
show run switch-profile
show switch-profile status
show switch-profile abc buffer
N3K-1(config-sync-sp)# buffer-delete all
```

## Notes
+ You need to starty with clean config.
+ If you tried to apply changes on config mode, is possible that you get an error.
```
N3K-2(config-sync)# show run switch-profile 

switch-profile abc
  sync-peers destination 10.88.171.23

  interface port-channel99
    switchport
    switchport mode trunk
    switchport trunk allowed vlan 1-3
N3K-2(config-sync)# show run int po 99

!Command: show running-config interface port-channel99
!Running configuration last done at: Thu Dec 23 18:24:07 2021
!Time: Thu Dec 23 18:24:20 2021

version 9.3(5) Bios:version 01.18 

interface port-channel99
  switchport
  switchport mode trunk
  switchport trunk allowed vlan 1-3

N3K-2(config-sync)# conf t
N3K-2(config)# int po 99
N3K-2(config-if)# switchport trunk allowed vlan 1-4
Error: Command is not mutually exclusive
N3K-2(config-if)# 
N3K-2(config)# no int po 99
Command is not mutually exclusive
```

+ if you want to delete a switch profile on one side:

```
N3K-2(config)# conf sync 
N3K-2(config-sync)# no switch-profile abc
% Invalid command

N3K-2(config-sync)# 
```
```
N3K-2(config-sync)# no switch-profile abc all-config 

WARNING: Deleting switch-profile will remove all commands configured under switch-profile. Are you sure you want to delete all switch-profile commands from the system ? 
Are you sure? (y/n)  [n] Y
```
N3K-1(config-sync)# show run int po99

!Command: show running-config interface port-channel99
!Running configuration last done at: Thu Dec 23 18:29:33 2021
!Time: Thu Dec 23 18:30:15 2021

version 9.3(8) Bios:version 01.20 

interface port-channel99

N3K-1(config-sync)# 
```
```
N3K-2(config-sync)# show run int po 99

!Command: show running-config interface port-channel99
!Running configuration last done at: Thu Dec 23 18:29:26 2021
!Time: Thu Dec 23 18:29:47 2021

version 9.3(5) Bios:version 01.18 

interface port-channel99

N3K-2(config-sync)# 
```
+ recovering previous config
+ To remove switch-profile but not the config use `no switch-profile abc profile-only local`

```
N3K-2(config-sync)# no switch-profile abc local-config 

WARNING: Deleting switch-profile will remove all commands configured under switch-profile. Are you sure you want to delete all switch-profile commands from the system ? 
Are you sure? (y/n)  [n] ^C
Are you sure? (y/n)  [n] Operation Failed: Execution was interrupted
N3K-2(config-sync)# no switch-profile abc ?
  all-config    Deletion of profile, local and peer configurations
  local-config  Deletion of profile and local configuration
  profile-only  Deletion of profile only and no other configuration

N3K-2(config-sync)# no switch-profile abc profile-only ?
  all    Deletion of profile only and no other configurations from all the
         peers 
  local  Deletion of profile only and no other configurations in local switch

N3K-2(config-sync)# no switch-profile abc profile-only local ?
  <CR>   

N3K-2(config-sync)# no switch-profile abc profile-only local

WARNING: Deleting switch-profile will not remove all commands configured under switch-profile. This will only remove the switch profile. Are you sure you want to delete the switch-profile from the system ? 
Are you sure? (y/n)  [n] Y
Please answer with "y" or "n"
invalid input
Are you sure? (y/n)  [n] y
Verification successful...
Proceeding to delete switch-profile. This might take a while depending on amount of configuration under a switch-profile.
Please avoid other configuration changes during this time.
Delete Successful
N3K-2(config-sync)# show run int po 99

!Command: show running-config interface port-channel99
!Running configuration last done at: Thu Dec 23 18:43:18 2021
!Time: Thu Dec 23 18:43:23 2021

version 9.3(5) Bios:version 01.18 

interface port-channel99
  switchport
  switchport mode trunk
  switchport trunk allowed vlan 1-3

N3K-2(config-sync)# 
  ```




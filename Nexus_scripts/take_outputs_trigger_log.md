+ This is a script that is trigger by an OSPF log.

```
event manager applet OSPF
  event syslog occurs 5 period 10 pattern "%OSPF-5-ADJCHANGE:"
  action 1.0 cli command "show clock >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 2.0 cli command "show ip ospf internal event-history flooding >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 3.0 cli command "show ip ospf internal event-history flooding >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 4.0 cli command "show ip ospf internal event-history spf  >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 5.0 cli command " show ip ospf internal event-history spf-trigger  >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 6.0 cli command "show ip ospf internal event-history adjacency >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 7.0 cli command "show ip ospf neighbors vrf all >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 8.0 cli command "show ip ospf database summary vrf all >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.0 cli command "show ip ospf database detail vrf all >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.1 cli command "show processes cpu sort >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.2 cli command "show processes cpu history >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.3 cli command "show system resources >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.4 cli command "show processes  memory >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
  action 9.5 cli command "show logging log >> bootflash:///$(SWITCHNAME)_ospf_logs.txt"
```

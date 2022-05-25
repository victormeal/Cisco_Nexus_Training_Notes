### EEM HIGH CPU script
```
event manager applet HIGH-CPU
  event snmp oid 1.3.6.1.4.1.9.9.109.1.1.1.1.6.1 get-type exact entry-op ge entry-val 70 poll-interval 10
  action 1.0 syslog priority critical msg HIGH CPU HIT $_event_pub_time
  action 2.0 cli enable
  action 3.0 cli show clock >> bootflash:high-cpu.txt
  action 4.0 cli show processes cpu sort | exclude 0.00 >> bootflash:high-cpu.txt
  action 5.0 cli show process cpu history >> bootflash:high-cpu.txt
  action 6.0 cli show policy-map interface control-plane | no-more >> bootflash:high-cpu.txt
  action 7.0 cli show hardware rate-limiter | no-more >> bootflash:high-cpu.txt
  action 8.0 cli show hardware internal cpu-mac inband counters >> bootflash:high-cpu.txt
  action 9.0 cli show system resources >> bootflash:high-cpu.txt
  action 10.0 cli show ip eigrp neighbor | no-more >> bootflash:high-cpu.txt
  action 11.0 cli show ip pim neighbor | no-more >> bootflash:high-cpu.txt
  action 12.0 cli show system internal mts buffer detail >> bootflash:high-cpu.txt
  action 13.0 cli show system internal mts buffer summary >> bootflash:high-cpu.txt
```
```
event manager applet HIGH-CPU-ethnalyzer
  event snmp oid 1.3.6.1.4.1.9.9.109.1.1.1.1.6.1 get-type exact entry-op ge entry-val 70 poll-interval 10
  action 1.0 cli ethanalyzer local interface inband limit-captured-frames 1000 write bootflash:high-cpu-eth.pcap
```

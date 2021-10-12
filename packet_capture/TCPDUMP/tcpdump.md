TCPDUMP

```
feature bash
run bash
sudo su

ifconfig
ifconfig Eth1-1

tcpdump -i Eth1-1
tcpdump -i Vlan1



tcpdump -A -i Eth1-1


bash-4.3# tcpdump -help
tcpdump version 4.7.4
libpcap version 1.6.2
OpenSSL 1.0.2d 9 Jul 2015
Usage: tcpdump [-aAbdDefhHIJKlLnNOpqRStuUvxX#] [ -B size ] [ -c count ]
                [ -C file_size ] [ -E algo:secret ] [ -F file ] [ -G seconds ]
                [ -i interface ] [ -j tstamptype ] [ -M secret ] [ --number ]
                [ -Q in|out|inout ]
                [ -r file ] [ -s snaplen ] [ --time-stamp-precision precision ]
                [ --immediate-mode ] [ -T type ] [ --version ] [ -V file ]
                [ -w file ] [ -W filecount ] [ -y datalinktype ] [ -z command ]
                [ -Z user ] [ expression ]
bash-4.3# 


tcpdump -i Eth1-1 -s 65535 -w test_capture_tcpdump.pcap
find / -name test_capture_tcpdump.pcap
```


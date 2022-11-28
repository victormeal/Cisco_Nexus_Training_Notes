# micro-flaps

```
NY7ASWB0220501# attach module 1
module-1# show system internal port-client link-event

*************** Port Client Link Events Log ***************
----                            ------        -----  -----  ------
Time                            PortNo        Speed  Event  Stsinfo
----                            ------        -----  -----  ------
Nov 25 19:30:58 2022  00648833  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 18:37:21 2022  00397830  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 18:01:23 2022  00056085  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 17:53:46 2022  00624241  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 17:42:38 2022  00264849  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 16:54:01 2022  00821599  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 15:09:37 2022  00904707  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 13:57:17 2022  00945765  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 13:03:17 2022  00022460  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 12:12:53 2022  00862306  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 12:07:54 2022  00621336  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 12:05:06 2022  00694147  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 11:14:30 2022  00170919  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 10:53:13 2022  00454543  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 10:36:41 2022  00748461  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 09:29:53 2022  00354672  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 09:15:43 2022  00905729  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 08:51:04 2022  00963438  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 08:36:03 2022  00034531  Ethernet1/34   40G   UP     SUCCESS(0x0)
Nov 25 08:35:49 2022  00432993  Ethernet1/34   ----  DOWN   Link down debounce timer stopped and link is down
Nov 25 08:35:49 2022  00317870  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 07:45:54 2022  00102487  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 07:26:05 2022  00264190  Ethernet1/34   40G   UP     SUCCESS(0x0)
Nov 25 07:25:53 2022  00413582  Ethernet1/34   ----  DOWN   Link down debounce timer stopped and link is down
Nov 25 07:25:53 2022  00299390  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 07:25:52 2022  00334264  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 05:03:57 2022  00845678  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 02:50:22 2022  00766215  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 25 00:49:46 2022  00680727  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 23:38:47 2022  00592779  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 18:11:17 2022  00352373  Ethernet1/33   40G   UP     SUCCESS(0x0)
Nov 24 18:05:21 2022  00972224  Ethernet1/33   ----  DOWN   Link down debounce timer stopped and link is down
Nov 24 18:05:21 2022  00857215  Ethernet1/33   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 14:44:09 2022  00687686  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 13:14:25 2022  00850016  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 12:56:54 2022  00131022  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 04:26:32 2022  00875424  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 03:28:54 2022  00189982  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 24 03:04:24 2022  00482686  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 22:51:21 2022  00407452  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 17:28:43 2022  00946718  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 13:55:44 2022  00085981  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 08:42:25 2022  00385102  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 06:08:07 2022  00774968  Ethernet1/34   ----  DOWN   Link down debounce timer started(0x40e50006)
Nov 23 03:42:50 2022  00390571  Ethernet1/4    40G   UP     SUCCESS(0x0)




In summary:
OSPF flaps are consequence of the microflaps in BV port E1/34
```

# Notes - "Nexus 7000 M- Series and F- Series Line Cards Introduction


1. There are two kind of I/O modules or Line Cards in Nexus - M Series and F Series. These are feature rich and feature specific cards.

2. F-Series were initially only Layer 2 supported. However, Now new Generation F Series Modules are also available such as F2,F2E,F3,F4 which support Routing as well.

3. M-Series support Layer 2 and Layer 3 capabilities. e.g. You will be able to run OSPF, EIGRP and BGP etc.

4. You can understand by naming convention of Line Cards to see whether these support N7K or N7700. E.g. N7K-M224XP-23L will only work with N7K and it is a M-Series Card.

5. Naming convention can be undertsood like this way N7K-M224XP-23L means It supports Nexus 7K, It is M Series, 2 means Second generation, 24 means It has max 24 ports. X means Max port capacity is 10 GE. P means support of SFP+. 2 Means It requires Fab2 module to function. 3 Means It requires minimum 3 FABRIC Modules to be able to work at full capacity. L means it supports Large number of ipv4, ipv6 route entries.

6. Fabric Bandwidth is the total bandwidth which ports of the modules can provide. e.g N7K-M224XP-23L has 24 x 10 GE ports . It means Fabric Bandwidth is 240Gbps.

7. Each M Series supports vPC ie. virtual port channel. VPC is known as Multi Chassis port channel.

8. FEX, QinQ , MPLS and OTV features are supported in all of the present M Series Modules.

9. All of the M Series Modules have Interoperability with F Series in the same VDC. It means if we have both M Series and F series interfaces in Same VDC, will there be any issues? Answer is No issues.

10. Similarly, you can get the details of F Series module from the cisco website as well. Naming Convention is similar to M Series.

11. FEX is supported with all F Series modules,

12. FabricPath is supported with all F series Modules

13. Layer 3 Interface is supported with all F Series Modules.

14. FCoE is supported with all F Series Modules.

15. M Series Interoperatibility is supported with F Series I/O cards.

16. Line Cards handle the data plane traffic that is why they are known as I/O Cards.
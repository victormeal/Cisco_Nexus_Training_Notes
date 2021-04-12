# Cisco Nexus Training - Udemy Course

## Notes - Nexus 7000 Chassis and Supervisor Engine Intro

1. Nexus Switches are product of Cisco.

2. To know about the Nexus models, open this website https://cisco.com/go/nexus

3. Nexus 9000 is the latest series which is ACI ( Application Centric Infrastructure)

4. ACI is also known as Cisco's SDN ( Software Defined Network)

5. Cisco Nexus 7000 is a Modular Series which means there will be  modules in the chassis and you can detach/attach these modules as well. It has two Sub-Series 7000 and 7700.

6. Nexus 7000 has four models i.e. 7004, 7009, 7010, 7018. Each Model supports redundant superivisors. Each module provide upto 550 Gbps bandwidth per slot.

7. Nexus 7700 has four models i.e. 7702, 7706, 7710 and 7718. These models also support redundant supervisors except 7702. Each module provides Upto 2.8Tbps bandwidth per slot.

8. There are two kind of modules used in 7000 Series Chassis. One is Supervisor Engine modules which are known as Forwarding Engines which manages the Control Plane as known as CPU. Second Ones are I/O Modules which are used for connecting Downlink/Uplink devices or servers.

9. If the Chassis model is 7004 series - It means Total number of modules will be 4. Out of 4, Two modules slots will be reserved for Supervisor Engine Modules and other 2 will be I/O.

10. Similarly if the chassis model is 7718, It means 2 Sup modules and 16 I/O Modules.

11. Chassis 7702 does not have sup redundancy because we can't connect Server or uplink/downlink switches if both are reserved for redundancy.

12. You can choose to connect only one supervisor in chassis, It is fine however, the Sup slot can't be used for connecting I/O module.

13. Also, You can't connect 7000 series Modules in nexus 7700 chassis and vice versa.

14. Switching Capacity- It means how much traffic or bits a chassis can handle in given period of time. e.g. 7004 switch has Switching Capacity of 1.92 Tbps. for 7018, Switching Capacity is 17.6Tbps.

15. These chassis support 1 GE, 10 GE, 40 GE and 100 GE ports

16. You need to know the Height of  these chassis so that these can fit in RACK.

17. RU is the Rack Unit and 1 RU = 1.75 inches of Height. while the width is standard across all vendors. e.g. 7009 needs 14 RU of space in order to keep it in Rack.

18. Each of these models have different Airflow . e.g. 7004 has Side-Rear and 7018 has Side-Side.

19. Side-Rear Airflow means Air Enters from one side and exits from Rear Side i.e. Back portion of chassis.

20. You can also download 3D App for viewing these models. This is really interactive app which will help you seeing the modules and other parts of these switches.

21. There are 3 varities of Supervisor Engines for 7000 Series chassis and 1 varity for 7700 Series. These are Nexus 7000 Sup 1 , Sup 2 , Sup 2E and Nexus 7700 Sup 2E.

22. sup 1 supports 4 VDC, Sup 2 supports 4 + 1 admin VDC, Sup 2 E supports 8 + 1 admin VDC. VDC stands for Virtual Device Context i.e. Logical way of dividing a physical chassis in to multiple switches.

23. Sup 1 and 2 Support up to 32 FEX while Sup 2E 7000 and Sup 2E 7700 has support for 64 FEX. FEX stands for Fabric Extender- It is a dumb device/Module used for connecting Servers. It uses the brain of the parent switch. Once FEX is connect to Parent Switch, then it appears as a Module in the Parent.
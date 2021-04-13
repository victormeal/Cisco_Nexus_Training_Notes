# Notes - "Understanding Naming Convention of Line Cards in Nexus 7000 "
## Important Points and Notes



### We can derive lot of information from the name of the Line Card.

#### We will take example of N7K-F248XT-25E



"N7K" Means this module is specifically designed for Nexus 7000 series Chassis. However, you should refer the datasheet of Particular chassis to make sure that this Line card is supported or not.

Not all of the N7K chassis may support it.

"F" means this line card is F-Series. If it was M then it will mean M Series.

"2" means It is second generation Line Card. In this case Second Generation F series I/O Module.

"48" means this Line Card will have total of 48 ports.

"X" means Largest port in terms of Bandwidth will be 10GE. Other options are F = 40 GE, C= 100 GE, G = 1 GE. This line card may have mix ports like 1G, 10G.

"T" here is port type = RJ45, Other options are 2= x2, K= Cisco CPAK, P =  SFP+, Q = QSFP+, S= SFP and T = RJ45

"2" here is the version of Fabric Module being used. There are two kind of fabric modules i.e. 1 and 2. Fabric module is an interconnect between the Line Cards. When you insert line cards in the chassis, then you also need to have Fabric Module.

FAB1 provides 46Gbps bandwidth while FAB2 provides 110Gbps. You need to make sure that you choose that fabric module which can let Line Cards function at its full capacity. e.g. if your Line card bandwidth is 110 , in that case, you can either use 3 FAB1 modules or 1 FAB2 module.

One Fabric Module provides bandwidth to each Line Card. If you have 7018 nexus chassis,  that means you have 16 line cards. If you are using FAB1 then it means this FABRIC module will serve 46Gbps to each of the 16 Line Cards.

You need to check datasheet to understand how many fab modules can fit in one chassis.

"5" here means that we should have 5 fabric modules for the card to operate at maximum capacity.

"E" here means It is F2E line card. There will be either "E" OR "L" or nothing. E is used only with F series Line Card. L means XL which means this line card has extended hardware resources like more ipv4/v6, more mac addresses, Access List etc.
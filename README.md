p1:  add physical interfaces to ospf
Step 1: Configure addressing and loopbacks
Rl# configure terminal
R1(config)# interface Loopback1
R1(config-if)# description Engineering Department
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# exit
Rl(config)# interface FastEthernet0/0
R1(config-if)# ip address 10.1.200.1 255.255.255.0
R1(config-if)# no shutdown
R2# conligure terminal
R2(config)# interface Loopback2
R2(config-if)# description Marketing Department
R2(config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# exit
R2(config)# interface FastEthernet0/0
R2(config-if)# ip address 10.1.200.2 255.255.255.0
R2(config-if)# no shutdown
R3# configure terminal
R3(config)# interface Loopback3
R3(config-if)# description Accounting Department
R3(config-if)# ip address 10.1.3.1 255.255.255.0
R3(config-if)# exit
R3(config)# interface FastEthernet0/0
R3(config-if)# ip address 10.1.200.3 255.255.255.0
R3(config-if)# no shutdown
R1(config)# interface Serial 0/0/0
R1(config-if)#f ip address 10.1.100.1 255.255.255.0
R1(config-if)# clockrate 64000
R1(config-if)#f bandwidth 64
R1(config-if)#f no shutdowna
R2(config)#f interface Serial 0/0/0
R2(config-if)# ip address 10.1.100.2 255.255.255.0
R2(config-i0)#₩ bandwidth 64
R2(config-if)# no shutdown

Step 2: Add physical interfaces to OSPF
Rl(config)# router ospf1
R1(config-router)# network 10.1.100,0 0.0,0.255 area 0
Rl(config-router)# network 10.1.200.0 0.0.0.255 area 0
Rl(config-router)# end
Rl#
R1# debug ip ospfadj Coptional)
OSPF adjacency events debugging is on
R2(config)# router ospf1
R2(config-router)# network 10.1.100.0 0.0.0.255 area 0
R2(config-router)# network 10.1.200.0 0.0.0.255 area 0
R3(config)# router ospf1
R3(config-router)# network 10.1.200.0 0.0.0.255 area 0
Rl#show ip protocols
Routing Protocol is "ospf 1"
Router ID 10.1.1.1
Number of areas in this router is 1. 1 normal 0 stub 0 nssa
Maximum path: 4
Routing for Networks:
10.1.100.0 0.0.0.255 area 0
10.1.200.1 0.0.0.0 area 0
Gateway Distance Last Update
Distance: (default is 110)
R1#show ip ospf
Routing Process "ospf 1" with ID 10.1.1.1
Rl# show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
10.1.2.1 1 FULL/BDR 00:00:36 10.1.200.2 FastEthernet0/0
10.1.3.1 1 FULL/DR 00:00:35 10.1.200.3 FastEthernet0/0
10.1.2.1 0 FULL/ - 00:00:36 10.1.100.2 Serial0/0/0
R1# show ip ospfinterface FastEthernet 0/0
R1# show ip ospf database
Step 4: Add loopback interfaces to OSPF.
R1,R2,R3# show ip route
R1(config)# routef ospf 1
R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R2(config)# router ospf 1
R2(config-router)# network 10.1.2.0 0.0.0.255 area 0
R3(config)# router ospf 1
R3(config-router)# network 10.1.3.0 0.0.0.255 area 0
R1,R2,R3# show ip route
10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
O 10.1.2.1/32 [1 10/2] via 10.1.200.2, 00:00:03, FastEthernet0/0
O 10.1.3.1/32 [110/2] via 10.1.200.3, 00:00:03, FastEthernet0/0
C10.1.1.0/24 is directly connected, Loopback

Rl# show ip ospfinterface Lo1
Loopbackl is up, line protocol is up
Internet Address 10.1.1.1/24, Area 0
Process ID 1, Router ID 10.1.1.1, Network 'Type LOOPBACK, Cost: 1
Loopback interface is treated as a stub Host
R1(config-if)# ip ospf network point-to-point
R2(config)# interface loopback2
R2(config-if)# ip ospf network point-to-point
R3(config)# interface loopback3
R3(config-if)# ip ospf network point-to-point
Rl# show ip route
Step 5: Modify OSPF link costs.
R1(config)# interface FastEthernet 0/0
R1(config-if)# ip ospf cost 50
R2(config)# interface FastEthernet 0/0
R2(config-if)# ip ospf cost 50
R3(config)# interface FastEthernet 0/0
R3(config-if)# ip ospf cost 50
R1# show ip route
10.0.0.0/24 is subnetted,5 subnets
O 10.1.3.0 [110/51] via 10.1.200.3, 00:01:40, FastEthernet0/0
O 10.1.2.0 [1 10/51] via 10.1.200.2, 00:01:40, FastEthernet0/0
Step 6: Modify interface priorities to control the DR and BDR election.
R1(config)# interface FastEthernet 0/0
R1(config-if)# ip ospf priority 10
R2(config)# interface FastEthernet 0/0
R2(config-if)# ip ospf priority 5
Rl# show ip ospf neighbor detail
Neighbor 10.1.2.1, interface address 10.1.200.2
In the area 0 via interface FastEthernet0/0
Neighbor priority is 5, State is FULL, 12 state changes
DR is 10.1.200.1 BDR is 10.1.200.2

1b:multi-area ospf  area n authenti

Step 1: Configure addressing and loopbacks.
R1# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R1(config)# interface loopback 1
R1(config-if)# deseription Engineering Department
R1(config-if)# ip address 10.1.1.1 255.255,255.0
R1(config-if)# interface serial 0/0/0
R1(config-if)# ip address 10.1.12.1 255.255.255,0
R1(config-if)# clockrate 64000
R1(config-if)# no shutdown
R2# configure terminal
R2(config)# interface loopback 2
R2(config-if)# description Marketing Department
R2(config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# interface serial 0/0/0
R2(config-if)# ip address 10.1.12.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface serial 0/0/1
R2(config-if)# ip address 10.1.23.2 255.255.255.0
R2(config-if)# clockrate 64000
R2(config-if)# no shutdown
R3# configure terminal
R3(config)# interface loopback 3
R3(config-if)# description Accounting Department
R3(config-if)# ip address 10.1.3.1 255.255.255.0
R3(config-if)# interface loopback 20
R3(config-if)# description Connection to another AS
R3(config-if)# ip address 172.20.200.1 255.255.255.0
R3(config-if)# interface serial 0/0/1
R3(config-if)# ip address 10.1.23.3 255.255.255.0
R3(config-if)# no shutdown
Step 2: Add interfaces into OSPF.
R1(config)# router ospf 1
R1(config-router)#f network 10.1.12.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R1(config-router)# exit
R1(config)# interface loopback 1
R1(config-if)# ip ospf network point-to-point
R2(config)# router ospf 1
R2(config-router)# network 10.1.12.0 0.0.0.255 area 0
R2(config-router)# network 10.1.2.0 0.0.0.255 area 0
R2(config-router)# exit
R2(config)/ interface 1oopback 2
R2(config-if)#ip ospf network point-to-point
R1,R2# show ip ospf neighbour
R1,R2# show ip route
R2(config)# router ospf 1
R2(config-router)# network 10.1.23.0 0.0.0.255 area 23
R3(config)# router Ospf 1
R3(config-router)# network 10.1.23.0 0.0.0.255 area 23
R3(config-router)# network 10.1.3.0 0.0.0.255 area 23
R3(config-router)#exit
R3(config)# interface loopback 3
R3(config-if)# ip ospf network point-to-point
R2#show ip ospf neighbour
R1# show ip route
O IA 10.1.3.0 [110/129] via 10.1.12.2, 00:00:28, Serial0/0/0
O 10.1.2.0 [110/65] via 10.1.12.2, 00:01:38, Serial0/0/0
O IA 10.1.23.0 [1 10/128] via 10.1.12.2, 00:01:38, Serial0/0/0
Step 3: Configure a stub area.
R2(config)# router ospf 1
R2(config-router)# area 23 stub
R3(config) router ospf 1
R3(config-router)# area 23 stub
R2,R3# show ip ospf neighbor
R3# show ip route
O IA 10.1.1.0 [110/129] via 10.1.23.2, 00:00:56, Serial0/0/1
C10.1.23.0 is directly connected, Serial0/0/1
O*IA 0.0.0.0/0 [110/65] via 10.1.23.2, 00:00:56, Serial0/0/1
R2# show ip ospf
Step 4: Configure a totally stubby area.
R3#show ip route
R2# show ip ospf database
R2(config)# router ospf 1
R2(config-router)# area 23 stub no-summary
R3# show ip route
O*IA 0.0.0.0/0 [110/65] via 10.1.23.2, 00:00:10, Serial0/0/1
R3#show ip ospf database
Summary Net Link States (Area 23)
Link ID ADV Router Age Seq# Checksum
0.0.0.0 10.1.2.1 68 0x80000002 0x0039F5

Step 5: Configure a not-so-stubby area.
R2(config)#router ospf1
R2(config-router)# no area 23 stub
R2(config-router)# area 23 nssa
R3(config)# router ospf1
R3(config-router)# no area 23 stub
R3(config-router)# area 23 nssa
R3(config-router)# redistribute connected subnets
R2# show ip ospf
Area 23
Number of interfaces in this area is I
It is a NSSA area
Perform type-7/type-5 LSA translation
Rl#show ip route
O E2 172.20.200.0 [1 10/20] via 10.1.12.2, 00:01:22, Serial0/0/0
R2(config)# router ospf 1
R2(config-router)# area 23 nssa no-summary
R3# show ip route
O*IA 0.0.0.0/0 [110/65] via 10.1.23.2, 00:00:20, Serial0/0/1
R2# show ip ospf database
Summary Net Link States (Area 23)
Link ID ADV Router Age Seq# Checksum
0.0.0.0 10.1.2.1 34 0x80000001 0x00C265
Type-7 AS External Link States (Area 23)
Link ID ADV Router Age Seq# Checksum Tag
10.1.3.0 10.1.3.1 200 0x80000001 0x0076FC O
Type-5 AS External Link States
Step 6: Configure OSPF interface authentication.
R2(config-if)# ip ospf authentication
R2(config-if)# ip ospf authentication-key cisco
R3(config)# interface serial 0/0/1
R3(config-if)# ip ospf authentication
R3(config-if)# ip ospf authentication-key cisco
R2# show ip ospf interface serial 0/0/1
Simple password authentication enabled
R1(config)# interface serial 0/0/0
R1(config-if)# ip ospf authentication message-digest
Rl(config-if)# ip ospf message-digest-key 1 md5 cisco
R2(config)# interface serial 0/0/0
R2(config-if)# ip ospf authentication message-digest
R2(config-if)#ip ospf message-digest-key 1 md5 cisco
Rl# show ip ospf interface serial 0/0/0
Message digest authentication enabled
Youngest key id is 1

p2: ospf virtual link n area summ

Step 1: Configure addressing and loopbacks.
R1# configure terminal
R1(config)# interface loopback 1
R1(config-if)# description Engineering Department
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-iD)# interface loopback 30
R1(config-if)#ip address 172.30.30.1 255.255.255.252
R1(config-if)# interface serial 0/0/0
R1(config-if)# ip address 10.1.12.1 255.255.255.0
R1(config-if)# clockrate 64000
R1(config-if)#no shutdown
R2# configure terminal
R2(config)# interface loopback 2
R2(config-if)# description Marketing Department
R2(config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# interface serial 0/0/0
R2(config-if)# ip address 10.1.12.2 255.255.255.0
R2(config-if)#no shutdown
R2(config-if)# interface serial 0/0/1
R2(config-if)# ip address 10.1.23.2 255.255.255.0
R2(config-if)# clockrate 64000
R2(config-if)# no shutdown
R3# configure terminal
R3(config)# interface loopback 3
R3(config-if)# description Accounting Department
R3(config-if)# ip address 10.1.3.1 255.255.255.0
R3(config-if)# interface loopback 100
R3(config-if)# ip address 192,168.100.1 255.255.255.0
R3(config-if)# interface loopback 101
R3(config-if)# ip address 192.168.101.1 255.255.255.0
R3(config-if)# interface loopback 102
R3(config-if)# ip address 192.168.102.1 255.255.255.0
R3(config-if)# interface loopback 103
R3(config-if)# ip address 192.168.103.1 255.255.255.0
R3(config-if)# interface serial 0/0/1
R3(config-if)# ip address 10.1.23.3 255.255.255.0
R3(config-if)# no shutdown
Step 2: Add interfaces into OSPF.
R1(config)# router ospf 1
R1(config-router)# network 10.1.12.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R1(config-router)# exit
R1(config)# interface loopback 1
R1(config-if)# ip ospf network point-to-point

R2(contig)# router ospf 1
R2(config-router)# network 10.1.12.0 0.0.0.255 area 0
R2(config-router)# network 10.1.2.0 0.0.0.255 area O
R2(config-router)#exit
R2(config)#interface loopback 2
R2(config-if)# ip ospf network point-to-point
R1,R2# show ip ospf neighbour
R1,R2# show ip route
R2(config)# router ospf 1
R2(config-router)# network 10.1.23.0 0.0.0.255 area 23
R3(config)# router ospf1
R3(config-router)# network 10.1.23.0 0.0.0.255 area area 23
R3(config-router)# network 10.1.3.0 0.0.0.255 area 23
R3(config-router)#exit
R3(config)#interface loopback 3
R3(config-if)# ip ospfnetwork point-to-point
R2# show ip ospf neighbour
Step 3: Create a virtual link.
R3(config)# router ospf 1
R3(config-router)# network 192.168.100.0 0.0.3.255 area 100
R3(config-router)#exit
R3(config)# interface loopback 100
R3(config-if)# ip ospf network point-to-point
R3(config-if)# interface loopback 101
R3(config-if)# ip ospf network point-to-point
R3(config-if)#interface loopback 102
R3(config-if)# ip ospf network point-to-point
R3(config-if)# interface loopback 103
R3(config-if)# ip ospf network point-to-point
R2# show ip route
R2#show ip ospf
Routing Process "ospf 1' with ID 10.1.2.1
R3# show ip ospf
Routing Process "ospf 1" with ID 192.168.103.1
R2(config)# router ospf 1
R2(config-router)# area 23 virtual-link 192.168.103.1
R3(config)# router ospf 1
R3(config-router)# area 23 virtual-link 10.1.2.1
R2# show ip route
R2# show ip ospf neighbour
R2# show ip ospf interface
OSPF_VL0 is up, line protocol is up
Step 4: Summarize an area.
R3(config)# router ospf1
R3(config-router)# area 100 range 192.168.100.0 255.255.252.0
R2# show ip route
R2# show ip ospf database
R3#show ip route
Step 5: Generate a default route into OSPF
R1(config)# router ospf 1
R1(config-router)# default-information originate always
R2,R3# show ip route
R3# ping 172.30.30.1

p3:a  redistri rip n ospf

Step 1: Configure loopbacks and assign addresses.
Rl (config)# interface Loopback0
Rl (config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# interface Loopback48
Rl (config-if)# ip address 192.168.48.1 255.255.255.0
Rl (config-if)# interface Loopback49
Rl (config-if)# ip address 192.168.49.1 255.255.255.0
Rl (config-if)# interface Loopback50
Rl (config-if)# ip address 192.168.50.1 255.255.255.0
R1 (config-if)# interface Loopback51
Rl (config-if)# ip address 192.168.51.1 255.255.255.0
Rl (config-if)# interface Loopback70
Rl (config-if)# ip address 192.168.70.1 255.255.255.0
Rl (config-if)# interface Serial0/0/0
Rl (config-if)# ip address 172.16.12.1 255.255.255.0
Rl(config-if)# clock rate 64000
Rl (config-if)# bandwidth 64
Rl (config-if)# no shutdown
R2(config)# interface Loopback0
R2 (config-if)# ip address 172.16.2.1 255.255.255.0
R2(config-if)# interface Serial0/0/0
R2(config-if)# ip address 172.16.12.2 255.255.255.0
R2(config-if)# bandwidth 64
R2(config-if)# no shutdown
R2(config-if)# interface Serial0/0/1
R2(config-if)# ip address 172.16.23.2 255.255.255.0
R2(config-if)# clock rate 64000
R2(config-if)# bandwidth 64
R2(config-if)# no shutdown
R3(config)# interface Loopback0
R3(config-if)# ip address 172.16.3.1 255.255.255.0
R3(config-if)#interface Loopback20
R3(config-if)# ip address 192.168.20.1 255.255.255.0
R3(config-if)#interface Loopback25
R3(config-if)# ip address 192.168.25.1 255.255.255.0
R3(config-if)# interface Loopback30
R3(config-if)# ip address 192.168.30.1 255.255.255.0
R3(config-if)# interface Loopback35
R3(config-if)# ip address 192.168.35.1 255.255.255.0
R3(config-if)# interface Loopback40
R3(config-if)# ip address 192.168.40.1 255.255.255.0
R3(config-if)#interface Serial0/0/1
R3(config-if)# ip address 172.16.23.3 255.255.255.0
R3(config-if)# bandwidth 64
R3(config-if)# no shutdown

Step 2: Configure RIPv2
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 172.16.0.0
R1(config-router)# network 192.168.48.0
R1(contfg-router# network 192.168.49.0
R1(config-router)# network 192.168.50.0
R1(config-router)# network 192.168.51.0
R1(config-router)# network 192.168.70.0
R2(config)# router rip
R2(config-router)# version 2
R2(config-router)#no auto-summary
R2(config-router)# network 172.16.0.0
R1,R2# show ip route rip
Rl,R2# show ip rip database
Step 3: Configure passive interfaces in RIP.
R1#show ip route rip
R2# show ip protocols
R2(config)# router rip
R2(config-router)# passive-interface serial 0/0/1
R2#show ip protocols
R1# show ip route rip
R1 (config)# router rip
R1 (config-router)# passive-interface loopback O
R1(config-router)# passive-interface loopback 48
R1 (config-router)# passive-interface loopback 49
R1 (config-router)# passive-interface loopback 50
R1 (config-router)# passive-interface loopback 51
R1 (config-router)# passive-interface loopback 70
R2 (config)# router rip
R2 (config-router)# passive-interface loopback C
OR (optional)
R1(config)# router rip
R1(config-router)# passive-interface default
R1(config-router)# no passive-interface Serial0/0/0
Step 4: Summarize a supernet with RIP.
R2#show ip route rip
R1(config)# interface serial 0/0/0
R1(config-if)# ip summary-address rip 192.168.48.0 255.255.252.0
Summary mask must be greater or equal to major net
R1 (config)# ip route 192.168.48.0 255.255.252.0 null0
R1 (config)# router rip
R1 (config/router)# redistribute static
R1# show ip route
R2# showip route

Step 5: Configure OSPF.
R2(config)#router ospf 1
R2(config-router)# network 172.16.23.0 0.0.0.255 area0
R3(config)#router ospf1
R3(config-router)# network 172.16.0.0 0.0.255.255 area0
R3(config-router)# network 192.168.0.0 0.0.255.255 area 0
R3 (config)#i interface Loopback0
R3 (config-if)t ip ospf network point-to-point
R3 (config-if)# interface Loopback20
R3 (config-if)f ip ospf network point-to-point
R3 (config-if)# interface Loopback25
R3(config-if)t ip ospf network point-to-point
R3(config-if)f interface Loopback30
R3 (config-if)f ip ospf network point-to-point
R3 (config-if)# interface Loopback35
R3(config-if)# ip ospf network point-to-point
R3(config-if)# interface Loopback40
R3(config-if)# ip ospf network point-to-point
R2# show ip ospf neighbour
R3# show ip ospf neighbour
R2# show ip route ospf
R3#show ip route ospf
Step 6: Configure passive interfaces in OSPF.
R3(config-router)# passive-interface loopback 0
R3(config)# router ospf I
R3 (config-router)# passive-interface default
R3 (config-router)# no passive-interface serial 0/0/1
R3# show ip protocols
Step 7: Allow one-way redistribution
R2(config)# router rip
R2 (config-router)# redistribute ospf I metric 4
R2# show ip protocols
Rl# show ip route rip
R1# ping 192.168.30.1.
R2 (config)# router ospf 1
R2 (config-router)# default-information originate
R3# show ip route ospf
Step 8: Redistribute between two routing protocols.
R2 (config)# router ospf I
R2 (config-router)# no default-information originate
R2 (config-router)# redistribute rip
8 Only classful networks will be redistributed
R3# show ip route ospf.
R2 (config)# router ospf 1
R2 (config-router)# redistribute rip subnets
R3# show ip route ospf
Step 9: Set a default seed metric.
R3# show ip route ospf
R2 (config)# router ospf I
R2 (config-router) #redistribute rip metric 10000
R3# show ip route ospf
Step 10: Change the OSPF external network type.
R2(config)# router ospf I
R2 (config-router)# redistribute rip metric-type I subnets
R3# show ip route ospf

p3: b: manipulating admin distances

Step 1: Configure router loopbacks and addressing.
Rl# conf t
R1(config)# interface loopback 1
R1(config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# interface loopback 101
R1(config-if)# ip address 192.168.101.1 255.255.255.0
Rl(config-if)# interface fastethernet 0/0
Rl(config-if)# ip address 172.16.12.1 255.255.255.0
Rl(config-if)# no shutdown
Rl(config-if)# interface serial 0/0/1
Rl(config-if)# bandwidth 64
Rl(config-if)# ip address 172.16.13.1 255.255.255.0
Rl(config-if)# no shutdown
R2# conft
R2(config)# interface loopback 2
R2(config-if)# ip address 172.16.2.1 255.255.255.0
R2(config-if)# interface loopback 102
R2(config-if)# ip address 192.168.102.1 255.255.255.0
R2(config-if)# interface fastethernet 0/0
R2(config-if)# ip address 172.16.12.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface fastethernet 0/1
R2(config-if)# ip address 172.16.23.2 255.255.255.0
R2(config-if)# no shutdown
R3# conft
R3(config)# interface loopback 3
R3(config-if)# ip address 172.16.3.1 255.255.255.0
R3(config-if)#interface loopback 103
R3(config-if)# ip address 192.168.103.1 255.255.255.0
R3(config-if)# interface fastethernet 0/0
R3(config-if)# ip address 172.16.23.3 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# interface serial 0/0/0
R3(config-if)# bandwidth 64
R3(config-if)# ip address 172.16.13.3 255.255.255.0
R3(config-if)# clock rate 64000
R3(config-if)# no shutdown

Step 2 : Configure switch VLANs.
Switch(config)# vlan 12
Switch(config-vlan)# name R1-R2
Switch(config-vlan)# vlan 23
Switch(config-vlan)# name R2-R3
Switch(config-vlan)# exit
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# description To R1 Fa0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 12
Switch(config-if)# interface fastEthernet 0/2
Switch(config-if)# description To R2 Fa0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 12
Switch(config-if)# interface fastEthernet 0/3
Switch(config-if)# description To R3 Fa0/0
Switch(config-if)# switchport mode access
Switch(config-if)#switchport access vlan 23
Switch(config-if)# interface fastEthernet 0/4
Switch(config-if)# description To R2 Fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 23
Step 3: Configure RIP
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 172.16.0.0
R1(config-router)# network 192.168.101.0
R2(config)# router rip
R2(config-router)# version 2
R2(config-router)# no auto-summary
R2(config-router)# network 172.16.0.0
R2(config-router)# network 192.168.102.0
R3(config)# router rip
R3(config-router)# version 2
R3(config-router)# no auto-summary
R3(config-router)# network 172.16.0.0
R3(config-router)# network 192.168.103.0
R1,R2,R3#show ip route rip
Rl,R2,R3# show ip protocols

Step4: Configure OSPF.
R1(config)# interface loopback 1
R1(config-if# ip ospf network point-to-point
R1(config-if#interface loopback 101
R1(config-if)# ip ospf network point-to-point
R1(config-if)# router ospf1
R1(config-router)# network 172.16.0.0 0.0.255.255 area 0
Rl(config-router)# network 192.168.101.0 0.0.0.255 area 0
R2(config)# interface loopback 2
R2(config-if)# ip ospf network point-to-point
R2(config-if)# interface loopback 102
R2(config-if)# ip ospf network point-to-point
R2(config-if)# router ospf 1
R2(config-router)# network 172.16.0.0 0.0.255.255 area 0
R2(config-router)# network 192.168.102.0 0.0.0.255 area 0
R3(config)# interface loopback 3
R3(config-if)# ip ospf network point-to-point
R3(config-if)# interface loopback 103
R3(config-if)# ip ospf network point-to-point
R3(config-if)# router ospf 1
R3(config-router)# network 172.16.0.0 0.0.255.255 area 0
R3(config-router)# network 192.168.103.0 0.0.0.255 area 0
.R1# show ip ospf neighbour
R1# show ip route
R2# show ip ospf neighbour
R2# show ip route
Step 5: Modify the routing protocol distance
Rl(config)# router rip
R1(config-router)# distance 100
R2(config)# router rip
R2(config-router)# distance 100
R3(config)# router rip
R3(config-router)# distance 100
Rl,R2,R3 # show ip route
Rl#show ip protocols
Step 7: Modify distance based on route source
R1(config)# router ospf 1
R1(config-router)# distance 85 192.168.100.0 0.0.3.255
R2(config)# router ospf 1
R2(config-router)# distance 85 192.168.100.0 0.0.3.255
R3(config)# router ospf 1
R3(config-router)# distance 85 192.168.100.0 0.0.3.255
R1,R2,R3# show ip route
Rl#show ip protocols
Step 8: Modify distance based on an access list.
R1(config)# access-list 1 permit 172.16.0.0 0.0.255.255
R1(config)# router rip
R1(config-router)# distance 65 0.0.0.0 255.255.255.255 1
R2(config)# access-list 1 permit 172.16.0.0 0.0.255.255
R2(config)# router rip
R2(config-router)# distance 65 0.0.0.0 255.255.255.255 1
R3(config)# access-list 1 permit 172.16.0.0 0.0.255.255
R3(config)# router rip
R3(config-router)# distance 65 0.0.0.0 255.255.255.255 1
R1,R2, R3# show ip route
R1r2,R3# show ip protocols

p4: a: bgp 

Step 1: Configure interface Addresses
Router 1
Router(config)#hostname ISP1
ISP1(config)#int Lo0
ISP1(config-if)#ip address 10.1.1.1 255.255.255.0
ISP1(config-if)#int s0/0/0
ISP1(config-if)#ip address 10.0.0.1 255.255.255.252
ISPI(config-if)# clock rate 128000
ISP1(config-if)#no shutdown
Router 2:
Router#conf t
Router(config)#hostname ITA
ITA(config)#int Lo0
ITA(config-if)#ip address 192.168.0.1 255255.255.0
ITA(config-if)#int Lo1
ITA(config-if)#ip address 192.168.1.1 255.255.255.0
ITA(config-if)#int s0/0/0
IT A(config-if)#ip address 10.0.0.2 255.255.255.252
IT A(config-if)#no shutdown
ITA(config-if)#int s0/0/1
ITA(config-if)#no-shutdown
ITA(config-if)#ip address 172.16.0.2 255.255.255.252
ITA(config-if)#clock rate 128000
This command applies only to DCE interfaces
ITA(config-if)#tend
ITA(config-if)#end

Router 3:
Router#conf t
Router(config)#hostname ISP2
ISP2(config)#int Lo0
ISP2(config-if)#ip address 172.16.1.1 255.255.255.0
ISP2(config-if)#int s0/0/1
ISP2(config-if)#ip address 172.16.0.1 255.255.255.252
ISP2(config-if)#no shutdown
Step 2:Configure BGP
Router 1:
ISP1>en
ISPl#conft
ISP1(config)#router bgp 200
ISP1(config-router)#neighbor 10.0.0.2 remote-as 100
ISP1(config-router)#network 10.1.1.0 mask 255.255.255.0
Router 3:
ISP2#conft
ISP2(config)#router bgp 300
ISP2(config-router)#neighbor 172.16.0.2 remote-as 100
ISP2(config-router)#network 172.16.1.0 mask 255.255.255.0
Router 2:
ITA>en
ITA#conf t
ITA(config)#router bgp 100
ITA(config-router)#neighbor 10.0.0.1 remote-as 200
IT A(config-router)#neighbor 172.16.0.1 remote-as 300
ITA(config-router)#network 192.168.0.0
ITA(config-router)#network 192.168.1.0
ITA#sh ip route
10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C 10.0.0.0/30 is directly connected, Serial0/0/0
B 10.1.1.0/24 20/01 via 10.0.0.1, 00:00:00
C172.16.0.0/30 is directly connected, Serial0/0/1
B 172.16.1.0/24 120/01 via 172.16.0.1, 00:00:00
Step 3: Verify bgp on the routers
Same on all router
ISP2#sh ip bgp
BGP table version is 5, local router ID is 172.16.1.1
Network Next Hop Metric LocPrf Weight Path
+*> 10.1.1.0/24 172.16.0.2000 100 200 i
*> 172.16.1.0/24 0.0.0.000: 32768
i
*> 192.168.0.0/24 172.16.0.2000 100i
+> 192.168.1.0/24 172.16.0.200 0 100i

Step 4:Configure primary and backup route using floating static route
Router 2:
ITA#conft
ITA(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1 210
ITA(config)#ip route 0.0.0.0 0.0.0.0 172.16.0.1 220
IT A#sh ip route
Gateway of last resort is 10.0.0.1 to network.0.0.0.0
Se 0.0.0.0/0 [210/01 via10.0.0.1
Router 1:
ISP1#conft
ISP1(config)#int Lo100
ISP1(config-if)#ip address 192.168.100.1 255.255.255.0
Router 2:
ITA#ping
Protocol [ip]:
Target IP address: 192.168.100.1
Repeat count [5]:
Datagram size [100]:
Timeout in seconds [2]:
Extended commands [n]:y
Source address or interface: 192.168.1.1
Type of service [0]:
Set DF bit in IP header? [no]:
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose[none]:
Sweep range of sizes [n]:
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.100.1, timeout is 2 seconds:
Packet sent with a source address of 192.168.1.1
!!!
Success rate is 100 percent (5/5), round-trip min/avg/max= 1/3/6 ms
Step 5: Configure primary and backup route using a default network and a
static route
Router 2:
ITA#conft
ITA(config)#no ip route 0,0.0.0 0.0.0.0 10.0.0.1 210
ITA(config)#no ip route 0.0.0.0 0.0.0.0 172.16.0.1 220
Router 1:
ISP1#conft
ISP1(config)#router bgp 200
ISP1(config-router)#network 192.168.100.0

Router 2:
ITA#sh ip route
Gateway of last resort is not set
C 192.168.1.0/24 is directly connected, Loopback1
B 192.168.100.0/24 20/01 via 10.0.0.1, 00:00:00
ITA#conft
ITA(config)#ip default-network 192.168.100.0
IT A#sh ip route
Gateway of last resort is 10.0.0.1 to network 192.168.100.0
C 192.168.1.0/24 is directly connected, Loopback1
B* 192.168.100.0/24 20/01 via 10.0.0.1. 00:00:00
ITA#conf t
ITA(config)#ip route 0.0.0.0 0.0.0.0 172.16.0.1 220
IT A#sh ip route
Gateway of last resort is 10.0.0.1 to network 192.168.100.0
B* 192.168.100.0/24 20/01 via 10.0.0.1. 00:00:00
S* 0.0.0.0/0 [220/01 via 172.16:0.1

p5: a:  ospf forr ipv6

Step 1: Configuring the hostname and loopback interfaces.
Router (config)# hostname R1
R1 (config)# interface loopback0
R1 (config-if)# ip address 10.1.1.1 255.255.255.0
R1 (config-if)# ipv6 address FEC0::1:1/112
Router (config)# hostname R2
R2 (config)# interface loopback0
R2 (config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# ipv6 address FEC0::2:1/112
Router (config)# hostname R3
R3(config)# interface loopback0
R3 (config-if)# ip address 10.1.3.1/ 255.255.255.0
R3 (config-if)# ipv6 address FEC0;:3:1/112
Step 2: Configure static IPv6 addresses.
R1 (config)# interface serial0/0/0
R1 (config-if)# ipv6 address FEC0::12:1/112
R1(config-if)# clockrate 64000
R1 (config-if)# bandwidth/64
R1 (config-if)# no shutdown
R1 (config-if)# interface serial0/0/1
R1 (config-if)# ipv6 address FEC0::13:1/112
R1 (config-if)# bandwidth 64
R1 (config-if)# no shutdown
R2(config)# interface serial0/0/0
R2 (config-if)# ipv6 address FEC0::12:2/112
R2 (config-if)# bandwidth 64
R2 (config-if)# no shutdown
R3 (config)# interface serial0/0/0
R3(config-if)# ipv6 address FEC0::13:3/112
R3 (config-if)# clockrate 64000
R3(config-if)# bandwidth 64
R3(config-if)# no shutdown
Rl# ping FEC0::12:2
R1# ping FECO::13:3
R2# ping FEC0::12:1
R3# ping FECO::13:1

Step 3: Change the link-local address on an interface.
R1# show ipv6 interface serial 0/0/0
R1 (config)# interface serial0/0/0
R1 (config-if)# ipv6 address FE80::1 link-local
R2 (config)# interface serial0/0/0
R2 (config-if)# ipv6 address FE80::2 link-local
R1# ping FE80::2
R2# ping FE80::1
R1#show ipv6 interface serial 0/0/0
R2#show ipv6 interface serial 0/0/0
Step 4: Configure EUI-64 addresses.
R2 (config)# interface fastEthernet 0/0
R2 (config-if)# ipv6 address FEC0:23::/64 eui-64
R2(config-if)# no shutdown
R3(config)# interface fastEthernet 0/0
R3(config-if)# ipv6 address FEC0:23::/64 eui-64
R3(config-if)#no shutdown
R2# show ipv6 interface fastEthernet 0/0
R3# show ipv6 interface brief
R2# ping FECO:23::218:B9FF:FECD:BEFO
Step 5: Enable IPv6 routing and CEF
R1 (config)# ipv6 unicast-routing
R1 (config)# ipv6 cef
R2 (config)# ipv6 unicast-routing
R2(config)# ipv6 cef
R3 (config)# ipv6 unicast-routing
R3 (config)# ipv6 cef
Step 6: Configure OSPFv3.
R1(config)# interface loopback0
R1(config-if)# ipv6 ospf I area 0
R1(config-if)# interface serial0/0/0
R1(config-if)# ipv6 ospf I area 0
R1 (config-if)# interface serial0/0/1
R1 (config-if)# ipv6 ospf I area 0
R2(config)# interface loopback0
R2(config-if)# ipv6 ospf I area 0
R2 (config-if)# interface serial0/0/0
R2 (config-if)# ipv6 ospf I area 0
R2 (config-if)# interface fastEthernet 0/0
R2 (config-if)# ipv6 ospf I area 0
R3 (config)# interface loopback0
R3 (config-if)# ipv6 ospf I area 0
R3(config-if)# interface serial0/0/0
R3 (config-if)# ipv6 ospf I area 0
R3(config-if)# interface fastEthernet 0/0
R3(config-if)# ipv6 ospf I area 0 ..-_.mea nenfneiahbor command.
b. Verify that you have OSPFy3 neighbors with the show ipv6 ospf neighbor
G
R1,R2,R3# show ipv6 ospf neighbor
R1,R2,R3# show ipv6 route
R1,R2,R3# show ípv6 ospf interface

p6:a vlans trunking n vtp domain modes

Step 1: Configure basic switch parameters.
Switch# configure terminal
Switch(config)# hostname DLS1
DLS1(config)#interface vlan 1
DLS1(config-if)# ip address 10.1.1.101 255.255.255.0
DLS1(config-if)# no shutdown
Switch# configure terminal
Switch(config)# hostname DLS2
DLS2(config)# interface vlan 1
DLS2(config-if)# ip address 10.1.1.102 255.255.255.0
DLS2(config-if)# no shutdown
Switch# configure terminal
Switch(config)# hostname ALS1
ALS1(config)# interface vlan 1
ALS1(config-if)# ip address 10.1.1.103 255.255.255.0
ALS1(config-if)# no shutdown
Switch# configure terminal
Switch(config)# hostname ALS2
ALS2(config)# interface vlan 1
ALS2(config-if)# ip address 10.1.1.104 255.255.255.0
ALS2(config-if)# no shutdown
Step 2: Display the switch default VLAN information.
ALS1# show vlan
Step 3: Examine VTP information.
DLS1# show vtp status
VIP Version
: running VTP1 (VTP2 capable)
Configuration Revision
:0
Maximum VLANs supported locally : 1005
Number of existing VLANs . S
VTP Operating Mode: Server

Step 4: Configure VTP on the switches.
DLS1(config)# vtp domain SWLAB
Changing VTP domain name from NULL to SWLAB
DLS1(config)# vtp version 2
DLS1(config)#l vtp mode server Device
mode already VTP SERVER.
ALS1(config)# vtp mode client Setting device
to VTP CLIENT mode.
ALS1# show vtp status
VTP Version
: running VTP1 (VTP2 capable)
VTP Operating Mode Client
Step 5: Configure trunking.
ALS1# show interfaces fastEthernet 0/7 switchport
Name: Fa0/7
Switchport: Enabled
Administrative Mode: dynamic: auto
DLS1(config)# interface range fastEthernetC 0/7-10
DLS1(config-if-range)# switchport trunk encapsulation dot1q
DLS1(config-if-range)#switchport mode trunk
DLS1(config)# interface range fastEthernet 0/11-12
DLS1(config-if-range)#switchport trunk encapsulation dot1q
DLS1(config-if-range)# switchport mode trunk
ALS1(config)#interface range fastEthernet 0/7-12
ALS2(config-if)#switchport mode trunk
ALS2(config)# interface range fastEthernet 0/7-12
ALS1(config-if)#switchport mode trunk
DLS2(config)# interface range fastEthernet 0/7 - 8
DLS2(config-if-range)#switchport trunk encapsulation dot1q
DLS2(config-if-range)#switchport mode trunk
Step 6: Verify trunk configuration.
ALS2# show interfaces fastEthernet 0/7 switchport
Name: Fa0/7
Trunking Native Mode VLAN: 1 (default)
Operational private-vlan: none Trunking VLANS
Enabled:ALL
DLS1 ,DLS2,ALS1,ALS2# show interfaces trunk
Port Mode
Encapsulation Status
Fa0/7 on
802.1q
trunking
1
Fao/8
on
802.1q
trunking
1
Fa0/9
on
802.1q
trunking
1
Fa0/10 on
802.1q
trunking
Step 7: Configure access ports.
ALS1(config)# interface fastEthernet 0/6
ALS1#(config-if)# switchport mode ?
ALS1(config)# interface fastEthernet 0/6
ALS1(config-if)#t switchport mode access
DLS1# show interfaces fastEthernet 0/6 switchport
Name:Fa0/6
Switchport: Enabled
Administrative Mode: static access
Negotiation of Trunking: Off

Step 8: Verify VTP configuration.
ALS1# show vtp status
VTP Version
:running VTP2
ALS2# show vtp status
3
Page
Step 9: Configure VLANs by assigning port membership.
DLS1(config)# interface FastEthernet 0/6
DLS1(config-if-range)# switchport access vlan 100%
DLS2(config)#interface FastEthernet 0/6
DLS2(config-if-range)# switchport access vlan 110
% Access VLAN does not exist. Creating vlan 110
DLS1# show vlan
100VLANO100 active Fa0/6
110VLAN0110 active
1002 fddi-default
Step 10: Configure VLANs in configuration mode.
DLS1(config)#vlan 120
ALS1(config)#interface fastEthernet 0/6
ALS1(config-if)# switchport access vlan 120
ALS2(config)# interface fastEthernet 0/6
ALS2(config-if)# switchport access vlan 120
ALS1#show vlan
Step 11: Change the VLAN names.
DLS1(config)# vlan 100
DLS1(config-vlan)# name Server-Farm-1
DLS1(config-vlan)# exit
DLS1(config)# vlan 110
DLS1(config-vlan)# name Server-Farm-2
DLS1(config-vlan)#exit
DLS1(config)# vlan120
DLS1(config-vlan)# name Net-Eng
DLS1(config-vlan)# exit
DLS1# show vlan
Gi0/2 100 Server-Farm-1 active fao/6
110 Server-Farm-2 active 120 Net-Eng
active
default
1002 fddi-default
act/unsup 1004 fddinet-default
act/unsup 1003 token-ring-
act/unsup
1005 trnet-default
act/unsup

p6:b: stp default behavior

Objective
Configure EtherChannel.
Step 1: Configure basic switch parameters.
DLS1# configure terminal
DLS1(config)# interface range fastEthernet 0/7 - 12
DLS1(config-if-range)#switchport trunk encapsulation dot1q
DLS1(config-if-range)# switchport mode trunk
DLS2#configure terminal
DLS2(config)# interface range fastEthernet 0/7 - 12
DLS2(config-if-range)# switchport trunk encapsulation dot1q
DLS2(config-if-range)# switchport mode trunk
ALS1# configure terminal
ALS1(config)# interface range fastEthernet 0/7- 12
ALS1(config-if-range)# switchport mode trunk
ALS2# configure terminal
ALS2(config)# interface range fastEthernet 0/7 -12
ALS2(config-if-range)# switchport mode trunk
Step 2: Configure an EtherChannel with Cisco PAgP.
ALS1# show interfaces trunk
ON ALS1 AND ALS2
ALS1(config)# interface range fastEthernet 0/11 - 12
ALS1(config-if-range)# channel-group 1 mode desirable
ALS1(config)# interface port-channel 1
ALS1(config-if)# switchport mode trunk
ALS1# show etherchannel summary
Number of channel-groups in use: 1
Number of aggregators:
1

Group Port-channel Protocol Ports
1 Po1(SU)
PAgP FaO/11(P) FaO/12(P)
ALS2#show etherchannel summary
ALS1# show interfaces trunk
Pol on 802.1q trunking
1
ALS1# show spanning-tree
Po1
Desg FWD 12
128.56 P2p
Step 3: Configure an 802.3ad LACP EtherChannel.
ALS1(config)# interface range fastEthernet 0/7-8
ALS1(config-if-range)# channel-group 2 mode active
Creating a port-channel interface Port-channel 2
ALS1(config-if-range)#interface port-channel2
ALS1(config-if)# switchport mode trunk.
ALS1# show etherchannel summary
1 Po2(SU) LACP FaO/7(P) FaO/8(P)
Step 4: Configure a Layer 3 EtherChannel..
DLS1(config)# interface range fastEthernet 0/11 - 12
DLS1(config-if-range)# no switchport
DLS1(config-if-range)# channel-group 3 mode desirable
Creating a port-channel interface Port-channel 3
DLS1(config-if-range)#interface port-channel 3
DLS1(config-if)# ip address 10.0.0.1 255.255.255.0
DLS2(config)# interface range fastEthernet 0/11 - 12
DLS2(config-if-range)#no switchport
DLS2(config-if-range)# channel-group 3 mode desirable
Creating a port-channel interface Port-channel 3
DLS2(config-if-range)# interface port-channel 3
DLS2(config-if)# ip address 10.0.0.2 255.255.255.0
DLS1# ping 10.0.0.2
DLS1# show etherchannel summary
Group Port-channel Protocol Ports
-+-
2 Po2(SU) LACP Fa0/7(P) FaO/8(P)
3 Po3(RU) PAgP Fa0/11(P) FaO/12(P)
Step 5: Configure load balancing.
DLS1# show etherchannel load-balance
ALS1(config)# port-channel load-balance src-dst-mac
ALS1# show etherchannel load-balance

p7: b:  modify default stp 

Objective
Observe what happens when the default spanning tree behavior is modified.
DLS1# configure terminal
DLS1(config)#interface range fastEthernet 0/7-12
DLS1(config-if-range)# switchport trunk encapsulation dot1q
DLS1(config-if-range)#switchport mode trunk
DLS2# configure terminal
DLS2(config)# interface range fastEthernet 0/7 -12
DLS2(config-if-range)# switchport trunk encapsulation dotlq
DLS2(config-if-range)#switchport mode trunk
ALS1# configure terminal
ALS1(config)# interface range fastEthernet 0/7- 12
ALS1(config-if-range)# switchport mode trunk
ALS2# configure terminal
ALS2(config)# interface range fastEthernet 0/7- 12
ALS2(config-if-range)# switchport mode trunk
Step 1: Display default spanning tree information for all switches.
PERFORM show spanning tree on all switches
DLS1# show spanning-tree
DSL1# show interfaces trunk
Step 2: Configure specific switches to be primary and secondary root.
Primary root switch.
DLS1(config)# spanning-tree vlan 1 root primary
Secondary root.
ALS1(config)# spanning-tree vlan 1 root secondary
DLS1# show run I include span
spanning-tree mode pvst spanning-tree extend system-id spanning-tree vlan 1 priority 2457

ALS1# show run I include span
spanning-tree mode pvst spanning-tree
extend system-id spanning-tree vlan 1
priority 28672
DLS1# show spanning-tree
Root ID Priority 24577
Address 00Oa.b8a9.d780
This bridge is the root
Step 3: Change the root port using the spanning-tree port-priority command.
DLS2# show spanning-tree
Fa0/11
Root FWD 19
128.13 P2p
Fa0/12 Altn BLK19 128.14 P2p
DLS1#show spanning-tree
This bridge is the root
Fa0/11 Desg FWD 19
28.13 P2p FaO/12
FWD 19 128.14 P2p
DLS1(config)# int fastEthernet 0/12
DLS1(config-if)# spanning-tree vlan 1 port-priority 112
DLS2# show spanning-tree
Desg
Port 14 (FastEthernet0/12)
Fa0/11
Altn BLK 19
128.13 P2p
Fa0/12 Root FWD19
128.14 P2p
DLS1# show spanning-tree
Fa0/11
Desg FWD 19
128.13 P2p
Step 4: Change root port using the spanning-tree cost command.
ALS2# show spanning-tree
Fa0/9
Root FWD 19
128.11 P2p
ALS2(config)# interface fastEthernet 0/10
ALS2(config-if-range)# spanning-tree vlan 1 cost 10
ALS2# show spanning-tree
P2p Fa0/10 Root FWD 10
28.12 P2p

p8: a : per vlan stp behave

DLS1 AND DLS2
DLS1(config)#interface range fastEthernet 0/7 - 12
DLS1(config-if-range)#switchport trunk encapsulation dot1q
DLS1(config-if-range)#switchport mode trunk
ALS1# configure terminal
ALS1(config)# interface range fastEthernet 0/7 - 12
ALS1(config-if-range)# switchport mode trunk
ALS2# configure terminal
ALS2(config)# interface range fastEthernet 0/7 - 12
ALS2(config-if-range)# switchport mode trunk
Step 1: Configure VTP.
DLS1(config)# vtp mode transparent
Setting device to VTP TRANSPARENT mode.
DLS1(config)# vtp domain CISCO
Changing VTP domain name from NULLtO CISCO
DLS1(config)# vlan 10
DLS1(config)# vlan 20
DLS1(config-vlan)#end
DLS1# show vlan brief.
DLS1# show spanning-tree
Spanning tree enabled protocol ieee Root ID
Priority3276
VLANOO10
VLANOO20

Step 3: Assign a root switch for each VLAN.
DLS1(config)# spanning-tree vlan 10 priority 4096
DLS2(config)# spanning-tree vlan 20 priority 4096
DLS1# show spanning-tree
VLANOO10
This bridge is the root
DLS2# show spanning-tree
VLANOOO1
This bridge is the root
VLANOO20
Spanning tree enabled protocol ieee oot ID
Priority 4116
Address 0OOa.b8a9.d680
This bridge is the root
ALS1# show spanning-tree
ALS2# show spanning-tree
Step 4: Configure RSTP.
DLS1(config)# spanning-tree mode rapid-pvst
DLS1# show spanning-tree
VLANOOO1
Spanning tree enabled protocol rstp Root ID
Priority 32769
Address 0OOa.b8a9.d680
Cost
19
Port13 (FastEthernet0/11)
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
VLANOO1O
Spanning tree enabled protocol rstp Root ID
Priority 4106
Address 00Oa.b8a9.d780
This bridge is the root
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
VLANOO20
Spanning tree enabled protocol rstp Root ID
Priority 4116
Address OOa,b8a9.d680


9A: INTER VLAN ROUT AN EXTERNAL R

Objective
Step 1: Configure the hosts.
Configure PC hosts A and B with the IP address, subnet mask (/24), and default gateway shown in
the topology
Step 2: Configure the routers.
Router(config)# hostname ISP
ISP(config)#interface Loopback0
ISP(config-if)# ip address 200.200.200.1 255.255.255.0
ISP(config-if)# interface Serial0/0/0
ISP(config-if)# ip address 192.168.1.2 255.255.255.0
ISP(config-if)#no shutdown
ISP(config-if)# exit
ISP(config)# ip route 172.16,0.0 255.255.0.0 192.168.1.1
Router(config)# hostname Gateway
Gateway(config)# interface Serial0/0/0
Gateway(config-if)# ip address 192.168.1.1 255.255.255.0
Gateway(config-if)#/clockrate 64000
Gateway(config-jf)# no shutdown
Gateway(config-if)#exit
Gateway(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.2
Step 3: Configure the switches.
Switch(config)# hostname ALS1
ALS1(config)# interface vlan 1
ALS1(config-if)# ip address 172.16.1.101 255.255.255.0
ALS1(config-if)# no shutdown
ALS1(config-if)#exit
ALS1(config)# ip default-gateway 172.16.1.1
The following is a sample configuration for switch ALS2
Switch(configl#hostname ALS2

ALS2(config)# interface vlan 1
ALS2(config-if)# ip address 172.16.1.102 255.255.255.0
ALS2(config-if)# no shutdown
ALS2(config-if)#exit
ALS2(config)# ip default-gateway 172.16.1.1
Step 4: Confirm the VLANS.
ALS1# show vlan
Step 5: Configure trunk links and EtherChannel on switches.
ALS1# configure terminal
ALS1(config)# interface range fastEthernet 0/11- 12
ALS1(config-if-range)# switchport mode trunk
ALS1(config-if-range)# channel-group 1 mode desirable
ALS1(config-if-range)# end
ALS2# configure terminal
ALS2(config)# interface range fastEthernet 0/11 - 12
ALS2(config-if-range)# switchport mode trunk
ALS2(config-if-range)# channel-group 1 mode desirable
ALS2(config-if-range)# end
b. ALS1# show etherchannel summary
1 Po1(SU)
PAgP FaO/11(P) FaO/12(P) Step 7:Configure VTP.
ALS2(config)# vtp mode client
Setting device to VTP CLIENT mode.
ALS1(config)# vtp domain SWLAB
Changing VTP domain name from NULL to SWLAB
ALS1(config)# vtp version 2
ALS1# show vtp status
ALS2# show vtp status
Configuration last modified by 172.16.1.101 at 2-28-10 00:36:24
Step 7: Configure VLANs and switch access ports.
ALS1(config)# vlan 100
ALS1(config-vlan)# name Payroll ALS1(config-vlan)# vlan
200
ALS1(config-vlan)# name Engineering
ALS2# show vlan brief
100 Payroll
active
200 Engineerin g
active
ALS1(config)#interface fastEthernet 0/6
ALS1(config-if)# switchport mode access
ALS1(config-if)# switchport access vlan 100
ALS1(config-if)# spanning-tree portfast
ALS2(config)# interface fastEthernet 0/6
ALS2(config-if)# switchport mode access
ALS2(config-if)# switchport access vlan 200
ALS2(config-if)# spanning-tree portfast
ALS1# show vlan brief
100 Payroll
active
F0/6

200 Engineering active
ALS2# show vlan brief
200 Engineering active Fa0/6
Step 9: Configure ALS1 trunking to the Gateway router.
ALS1(config)#interface fastEthernet 0/1
ALS1(config-if)# switchport mode trunk
ALS1(config-if)#end
Note: Optionally, you can apply the spanning-tree portfast trunk command to interface Fa0/1
of switch ALS1. This allows the link to the router to rapidly transition to the forwarding state
despite being a trunk.
Step 10: Configure the Gateway router Fast Ethernet interface for VLAN trunking.
Gateway(config)# interface fastEthernet 0/0C Gateway(config-if)# no shut
Gateway(config)#interface fastEthernet 0/0.1
Gateway(config-subif)# description Management VLAN 1
Gateway(config-subif)#encapsulation dot1q 1 native
Gateway(config-subif)# ip address 172.16.1.1 255.255.255.0
Gateway(config-subif)# interface fastEthernet 0/0.100
Gateway(config-subif)# description Payroll VLAN 100
Gateway(config-subif)# encapsulation dot1q 100
Gateway(config-subif)# ip address 172.16.100.1 255.255.255.0
The following is a sample configuration for the VLAN 200 subinterface.
Gateway(config-subif)# interface fastEthernet 0/0.200
Gateway(config-subif)# description Engineering VLAN 200
Gateway(config-subif)# encapsulation dot1q 200
Gateway(config-subif)# ip address 172.16.200.1 255.255.255.0
Gateway(config-subif)#end
Use the show ip interface brief command to verify the interface configuration and status.
Gateway# show ip interface brief
Interface IP-Address OK? Method Status
Protocol
FastEthernet0/0 unassigned YES unset pup
FastEthernet0/1.1 172.16.1.1 YES manual up
up
FastEthernet0/1.100 172.16.100.1 YES manual up
up
FastEthernet0/1.200 172.16.200.1 YES manual up
up
Gateway# show interfaces description
Fa0/0.1
upup ManagementVLAN 1
Fa0/0.100
upup PayrollVLAN 100
Fao/0.200
upuap Engineering VLAN 200
Gateway# show vlans

p9: inter -vlan rout n internal route cef

1: Configure basic switch parameters.
Switch(config)# hostname ALS1
ALS1(config)# enable secret cisco
ALS1(config)# line vty 0 15
ALS1(config-line)# password cisco
ALS1(config-line)# login
Switch(config)# hostname ALS2
ALS2(config)# enable secret cisco
ALS2(config)#line vty 0 15
ALS2(config-line)# password cisco
ALS2(config-line)# login
Switch(config)# hostname DLS1
DLS1(config)# enable secretcisco
DLS1(config)# line vty 0 15
DLS1(config-line)#password cisco DLS1(config-line)# login
ALS1(config)# interface vlan 1
ALS1(config-if)# ip address 172.16.1.101 255.255.255.0
ALS1(config-if)# no shutdown
ALS2(config)# interface vlan 1
ALS2(config-if)# ip address 172.16.1.102 255.255.255.0

ALS2(config-if)# no shutdown
DLS1(config)# interface vlan 1
DLS1(config-if)# ip address 172.16.1.1 255. 255. 255.0 DLS1(config-if)# no shutdown
ALS1(config)# ip default-gateway 172.16.1.1
ALS2(config)# ip default-gateway 172.16.1.1
Step 2: Configure trunks and EtherChannels between switches.
DLS1(config)# interface range fastEthernetC 0/7 -8
DLS1(config-if-range)# switchport trunk encapsulation dot1q
DLS1(config-if-range)# switchport mode trunk
DLS1(config-if-range)# channel-group 1 mode desirable
Creating a port-channel interface Port-channel 1
DLS1(config)# interface range fastEthernet 0/9- 10
DLS1(config-if-range)# switchport trunk encapsulation dot1q
DLS1(config-if-range)# switchport mode trunk
DLS1(config-if-range)# channel-group 2 mode desirable
Creating a port-channel interface Port-channel 2
ALS1(config)# interface range fastEthernet 0/11-12
ALS1(config-if-range)# switchport mode trunk
ALS1(config-if-range)# channel-group 1 mode desirable
Creating a port-channel interface Port-channel 1
ALS1(config-if-range)# exit
ALS1(config)# interface range fastEthernet 0/7 -8
ALS1(config-if-range)# switchport mode trunk
ALS1(config-if-range)# channel-group 2 mode desirable
Creating a port-channel interface Port-channel 2
ALS2(config)# interface range fastEthernet 0/11 - 12
ALS2(config-if-range)# switchport mode trunk
ALS2(config-if-range)# channel-group 1 mode desirable
Creating a port-channel interface Port-channel 1
ALS2(config-if-range)# exit
ALS2(config)# interface range fastEthernet 0/9 - 10
ALS2(config-if-range)# switchport mode trunk
ALS2(config-if-range)# channel-group 2 mode desirable
Creating a port-channel interface Port-channel 2
DLS1# show interface trunk
ALS1# show etherchannel summary
Step 3: Configure VTP on ALS1 and ALS2.
ALS1(config)#vtp mode client
Setting device to VTP CLIENT mode.

ALS2(config)#vtp mode client
Setting device to VTP CLIENT mode.
ALS2# show vtp status
VTP Version : running VTP1 (VTP2 capable)
Configuration Revision :O
Maximum VLANs supported locally : 255
Number of existing VLANs
S
VTP Operating Mode
Client
Step 4: Configure VTP on DLS1.
DLS1(config)# vtp domain SWPOD
DLS1(config)# vtp version 2
DLS1(config)# vlan 100
DLS1(config-vlan)# name Finance
DLS1(config-vlan)# vlan 200
DLS1(config-vlan)# name Engineering
Step 5: Configure ports.
ALS1(config)# interface fastEthernet 0/6
ALS1(config-if)# switchport mode access
ALS1(config-if)# switchport access vlan 100
ALS2(config)# interface fastEthernet 0/6
ALS2(config-if)# switchport mode access
ALS2(config-if)# switchport access vlan 200
Step 6: Configure VLAN interfaces and enable routing
DLS1(config)# interface vlan 100
DLS1(config-if)# ip add 172.16.100.1 255.255.255.0
DLS1(config-if)# no shut
DLS1(config-if)# interface vlan 200
DLS1(config-if)# ip address 172.16.200.1 255.255.255.0 DLS1(config-if)# no shutdown
DLS1(config)# ip routing
DLS1# show ip route

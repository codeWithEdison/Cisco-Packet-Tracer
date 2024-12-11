# Lab 2-1: EIGRP Configuration, Bandwidth, and Adjacencies
## Lab Report

### Learning Objectives Completed
1. Configured EIGRP on interfaces
2. Configured bandwidth command to limit EIGRP bandwidth
3. Verified EIGRP adjacencies
4. Verified EIGRP routing information exchange
5. Used debugging commands for EIGRP troubleshooting
6. Tested convergence during topology changes

### Initial Network Setup

#### Network Topology
- R1: Engineering Department
  - Loopback1: 10.1.1.1/24
  - FastEthernet0/0: 10.1.100.1/24
  - Serial0/0/0: 10.1.200.1/24 (Connected to R2)
  
- R2: Marketing Department
  - Loopback2: 10.1.2.1/24
  - FastEthernet0/0: 10.1.100.2/24
  - Serial0/0/0: 10.1.200.2/24
  
- R3: Accounting Department
  - Loopback3: 10.1.3.1/24
  - FastEthernet0/0: 10.1.100.3/24
  - Loopback11: 192.168.100.1/30
  - Loopback15: 192.168.100.5/30

### Configuration Steps

#### 1. Initial Router Configurations

```cisco
! R1 Configuration
R1#configure terminal
R1(config)# interface Loopback1
R1(config-if)# description Engineering Department
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# exit
R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 10.1.100.1 255.255.255.0
R1(config-if)# no shutdown
```

```cisco
! R2 Configuration 
R2#configure terminal
R2(config)# interface Loopback2
R2(config-if)# description Marketing Department
R2(config-if)# ip address 10.1.2.1 255.255.255.0
R2(config-if)# exit
R2(config)# interface FastEthernet0/0
R2(config-if)# ip address 10.1.100.2 255.255.255.0
R2(config-if)# no shutdown
```

```cisco
! R3 Configuration
R3#configure terminal
R3(config)# interface Loopback3
R3(config-if)# description Accounting Department
R3(config-if)# ip address 10.1.3.1 255.255.255.0
R3(config-if)# exit
R3(config)# interface FastEthernet0/0
R3(config-if)# ip address 10.1.100.3 255.255.255.0
R3(config-if)# no shutdown
```

#### 2. EIGRP Configuration

```cisco
! R1 EIGRP Configuration
R1(config)# router eigrp 1
R1(config-router)# network 10.0.0.0
R1(config-router)# auto-summary

! R2 EIGRP Configuration
R2(config)# router eigrp 1
R2(config-router)# network 10.0.0.0
R2(config-router)# auto-summary

! R3 EIGRP Configuration
R3(config)# router eigrp 1
R3(config-router)# network 10.0.0.0
R3(config-router)# network 192.168.100.0 0.0.0.3
R3(config-router)# auto-summary
```

#### 3. Serial Interface Configuration

```cisco
! R1 Serial Interface
R1(config)# interface Serial0/0/0
R1(config-if)# ip address 10.1.200.1 255.255.255.0
R1(config-if)# clock rate 64000
R1(config-if)# bandwidth 64
R1(config-if)# no shutdown

! R2 Serial Interface
R2(config)# interface Serial0/0/0
R2(config-if)# ip address 10.1.200.2 255.255.255.0
R2(config-if)# bandwidth 64
R2(config-if)# no shutdown
```

### Verification Steps

1. **Verify EIGRP Neighbors**
```cisco
R1# show ip eigrp neighbors
IP-EIGRP neighbors for process 1
H   Address         Interface      Hold  Uptime    SRTT   RTO   Q   Seq
                                  (sec)            (ms)         Cnt  Num
2   10.1.200.2     Se0/0/0       10    00:03:03   24    200   0   53
1   10.1.100.2     Fa0/0         14    09:22:42   269   1614  0   54
0   10.1.100.3     Fa0/0         11    09:22:42   212   1272  0   59
```

2. **Verify EIGRP Topology**
```cisco
R1# show ip eigrp topology
IP-EIGRP Topology Table for AS(1)/ID(10.1.1.1)

P 10.1.3.0/24, 1 successors, FD is 156160
    via 10.1.100.3 (156160/128256), FastEthernet0/0
P 10.1.2.0/24, 1 successors, FD is 156160
    via 10.1.100.2 (156160/128256), FastEthernet0/0
```

3. **Verify EIGRP Interfaces**
```cisco
R1# show ip eigrp interfaces
IP-EIGRP interfaces for process 1

Interface    Peers    Xmit Queue   Mean    Pacing Time    Multicast    Pending
                     Un/Reliable   SRTT    Un/Reliable    Flow Timer   Routes
Fa0/0        2       0/0          0       0/1            0            0
Se0/0/0      1       0/0          0       0/1            0            0
```

### Testing Convergence
1. Initiated ping from R3 to 10.1.1.1 with repeat count 100000
2. Shut down R1's FastEthernet0/0 interface
3. Observed route reconvergence via serial link
4. Verified new path with traceroute:
```cisco
R3# traceroute 10.1.1.1
Type escape sequence to abort.
Tracing the route to 10.1.1.1
1 10.1.100.2 0 msec 4 msec 0 msec
2 10.1.200.1 12 msec * 12 msec
```

### Key Findings
1. EIGRP established adjacencies successfully between all routers
2. Bandwidth configuration on serial interfaces properly limited EIGRP traffic
3. Network converged quickly when FastEthernet link failed
4. Wildcard masks correctly implemented for network statements
5. All department loopbacks accessible through EIGRP routing

### Conclusion
All learning objectives were successfully achieved. The network demonstrated proper EIGRP operation, including:
- Fast convergence during topology changes
- Proper bandwidth utilization on serial links
- Correct route advertisement and path selection
- Successful debugging and verification procedures

The lab provided practical experience in EIGRP configuration and troubleshooting in a multi-router environment.

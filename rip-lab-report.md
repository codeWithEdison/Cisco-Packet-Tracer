# Lab Report: Configuring RIP (Routing Information Protocol)
## Student Lab Report

### Lab Overview
**Objective**: Configure and verify RIP routing protocol operation between multiple Cisco routers

**Equipment Used**:
- 3 Cisco 2600 Series Routers (R1, R2, R3)
- 2 PCs (PC1, PC2)
- Console cables
- Ethernet cables

### Network Topology
![Network Topology]
- R1: FastEthernet 0/0 (192.1.1.1)
- R2: FastEthernet 0/0 (192.1.1.2), FastEthernet 0/1 (193.1.1.1)
- R3: FastEthernet 0/1 (193.1.1.2)

### Step-by-Step Configuration Process

#### Initial Router Configuration

1. **Router R1 Configuration**:
```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret cisco
R1(config)# interface fastethernet 0/0
R1(config-if)# ip address 192.1.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

2. **Router R2 Configuration**:
```
Router> enable
Router# configure terminal
Router(config)# hostname R2
R2(config)# enable secret cisco
R2(config)# interface fastethernet 0/0
R2(config-if)# ip address 192.1.1.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface fastethernet 0/1
R2(config-if)# ip address 193.1.1.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit
```

3. **Router R3 Configuration**:
```
Router> enable
Router# configure terminal
Router(config)# hostname R3
R3(config)# enable secret cisco
R3(config)# interface fastethernet 0/1
R3(config-if)# ip address 193.1.1.2 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit
```

#### RIP Protocol Configuration

1. **Configuring RIP on R1**:
```
R1(config)# router rip
R1(config-router)# network 192.1.1.0
R1(config-router)# exit
```

2. **Configuring RIP on R2**:
```
R2(config)# router rip
R2(config-router)# network 192.1.1.0
R2(config-router)# network 193.1.1.0
R2(config-router)# exit
```

3. **Configuring RIP on R3**:
```
R3(config)# router rip
R3(config-router)# network 193.1.1.0
R3(config-router)# exit
```

### Verification Steps

1. **Verifying RIP Configuration**:
- Used `show ip protocols` to verify RIP configuration
- Confirmed routing updates being sent and received
- Verified network statements are correct

2. **Checking Routing Tables**:
```
R2# show ip route
```
Output showed:
- Direct connections (C)
- RIP-learned routes (R)
- Proper next-hop addresses

3. **Testing Connectivity**:
- Successfully pinged between routers
- Verified end-to-end connectivity through network

### RIP Debugging and Monitoring

1. **Viewing RIP Updates**:
```
R2# debug ip rip
```
Observed:
- Regular routing updates every 30 seconds
- Route advertisements from neighboring routers
- Metric calculations

2. **Testing Split-Horizon**:
```
R3(config)# interface fastethernet 0/1
R3(config-if)# no ip split-horizon
```
Results:
- Verified route propagation
- Confirmed proper update behavior

### Additional Tasks Completed

1. **Triggered Updates**:
- Cleared routing table using `clear ip route *`
- Observed immediate RIP updates
- Verified rapid convergence

2. **Poison Reverse Testing**:
- Shut down interfaces to test poison reverse
- Monitored route removal process
- Verified proper network convergence

### Conclusion
Successfully configured RIP routing between three routers. Verified proper operation through:
- Routing table examination
- Debug output analysis
- Connectivity testing
- Split-horizon configuration
- Poison reverse verification

All objectives were met and the network is functioning as expected.

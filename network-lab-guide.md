# Network Lab Setup Guide

## Equipment Required
- 2 Cisco 2600 Series Routers
- 2 Quidway S3900 Series Switches
- 2 PCs with 10/100 NICs
- 4 Straight-through cables
- 2 Console (rollover) cables
- 1 Serial cable
- HyperTerminal/PuTTY software

## Lab Overview
This guide will walk you through setting up a basic network consisting of two routers connected via a serial link, with each router connected to a switch and workstation. You'll configure static routing between the networks.

## Network Topology
- CapeTown Router:
  - FastEthernet 0/0: 192.168.4.1/24
  - Serial 0/0: 192.168.1.1/24
- Durban Router:
  - FastEthernet 0/0: 192.168.8.1/24
  - Serial 0/0: 192.168.1.2/24
- Workstation 1: 192.168.4.2/24
- Workstation 2: 192.168.8.2/24

## Step-by-Step Configuration

### 1. Physical Setup
1. Connect devices according to the network diagram:
   - Connect each router's FastEthernet 0/0 to a switch
   - Connect workstations to their respective switches
   - Connect the routers' Serial 0/0 ports using the serial cable
   - Connect console cables from PCs to routers' console ports

### 2. Workstation Configuration
1. Configure Workstation 1:
   - IP: 192.168.4.2
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.4.1

2. Configure Workstation 2:
   - IP: 192.168.8.2
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.8.1

### 3. CapeTown Router Configuration
1. Enter privileged mode:
```
Router> enable
Router# configure terminal
```

2. Set basic configuration:
```
Router(config)# enable secret cisco
Router(config)# hostname CapeTown
```

3. Configure FastEthernet interface:
```
CapeTown(config)# interface fastethernet 0/0
CapeTown(config-if)# ip address 192.168.4.1 255.255.255.0
CapeTown(config-if)# no shutdown
CapeTown(config-if)# exit
```

4. Configure Serial interface:
```
CapeTown(config)# interface serial 0/0
CapeTown(config-if)# ip address 192.168.1.1 255.255.255.0
CapeTown(config-if)# no shutdown
```

5. Check if DCE cable is connected and set clock rate if needed:
```
CapeTown# show controllers serial 0/0
CapeTown(config-if)# clock rate 56000    (only if DCE)
```

### 4. Durban Router Configuration
1. Enter configuration mode:
```
Router> enable
Router# configure terminal
Router(config)# enable secret cisco
Router(config)# hostname Durban
```

2. Configure FastEthernet interface:
```
Durban(config)# interface fastethernet 0/0
Durban(config-if)# ip address 192.168.8.1 255.255.255.0
Durban(config-if)# no shutdown
Durban(config-if)# exit
```

3. Configure Serial interface:
```
Durban(config)# interface serial 0/0
Durban(config-if)# ip address 192.168.1.2 255.255.255.0
Durban(config-if)# no shutdown
```

### 5. Configure Static Routes
1. On CapeTown Router:
```
CapeTown(config)# ip route 192.168.8.0 255.255.255.0 192.168.1.2
```

2. On Durban Router:
```
Durban(config)# ip route 192.168.4.0 255.255.255.0 192.168.1.1
```

### 6. Verification Steps
1. Check interface status:
```
show ip interface brief
```

2. Verify routing tables:
```
show ip route
```

3. Test connectivity:
- From Workstation 1, ping:
  - CapeTown Router (192.168.4.1)
  - Workstation 2 (192.168.8.2)
- From Workstation 2, ping:
  - Durban Router (192.168.8.1)
  - Workstation 1 (192.168.4.2)

### Troubleshooting Tips
1. If interfaces are down:
   - Check cable connections
   - Verify "no shutdown" is configured
   - Confirm IP addresses are correctly set

2. If pings fail:
   - Verify static routes are configured correctly
   - Check IP addresses and subnet masks
   - Confirm default gateways on workstations

3. If serial link issues:
   - Verify clock rate is set on DCE end
   - Check cable connections
   - Confirm matching subnet masks

### Save Configurations
Remember to save configurations on both routers:
```
Router# copy running-config startup-config
```
or
```
Router# write memory
```

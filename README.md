# Inter-VLAN Routing Lab

## Overview
This project demonstrates **Inter-VLAN Routing** using the **Router-on-a-Stick** method in Cisco Packet Tracer.  
It shows how to segment a network into multiple VLANs and enable communication between them through a single physical router interface with subinterfaces.

## Objectives
- Create and name multiple VLANs
- Assign access ports to VLANs
- Configure an 802.1Q trunk link between the switch and the router
- Configure router subinterfaces (Router-on-a-Stick)
- Assign IP addresses and default gateways
- Verify inter-VLAN connectivity

## Network Topology

### Device List
| Device     | Model       | Role                          |
|------------|-------------|-------------------------------|
| Router1    | PT-8200     | Inter-VLAN Router (Router-on-a-Stick) |
| Switch0    | 2950T-24    | Access + Distribution Switch  |
| Switch1    | 2950T-24    | Access Switch                 |
| 2 × Green PCs  | PC-PT   | VLAN 10 hosts                 |
| 2 × Yellow PCs | PC-PT   | VLAN 20 hosts                 |

## VLAN Design

![image alt]

| VLAN ID | Color  | Name / Purpose     | Devices                          |
|---------|--------|--------------------|----------------------------------|
| 10      | Green  | VLAN 10            | 130.34.12.66 and 130.34.12.67    |
| 20      | Yellow | VLAN 20            | 130.34.12.82 and 130.34.12.83    |

## IP Addressing Scheme

| Device              | Interface / Port | IP Address     | VLAN  | Default Gateway   |
|---------------------|------------------|----------------|-------|-------------------|
| Router1             | Gig0/0/0.10      | 130.34.12.1    | 10    | -                 |
| Router1             | Gig0/0/0.20      | 130.34.12.65   | 20    | -                 |
| PC (Green - Left)   | Fa0              | 130.34.12.66   | 10    | 130.34.12.1       |
| PC (Green - Right)  | Fa0              | 130.34.12.67   | 10    | 130.34.12.1       |
| PC (Yellow - Left)  | Fa0              | 130.34.12.82   | 20    | 130.34.12.65      |
| PC (Yellow - Right) | Fa0              | 130.34.12.83   | 20    | 130.34.12.65      |

## Configuration

### 1. Switch0 Configuration

enable
configure terminal

! Create VLANs
vlan 10 (GREEN)
 
vlan 20 (YELLOW)

exit

! Access ports
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
 no shutdown

interface FastEthernet0/3
 switchport mode access
 switchport access vlan 20
 no shutdown

! Trunk to Router
interface GigabitEthernet0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

! Trunk to Switch1
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

end
write memory

### 2. Switch1 Configuration

enable
configure terminal

! Create VLANs
vlan 10 (GREEN)
 
vlan 20 (YELLOW)
 
exit

! Access ports
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
 no shutdown

interface FastEthernet0/3
 switchport mode access
 switchport access vlan 20
 no shutdown

! Trunk to Switch0
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

end
write memory

### Router1 configuration (router-on-a-stick)

enable
configure terminal

interface GigabitEthernet0/0/0
 no ip address
 no shutdown

! Subinterface for VLAN 10
interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 130.34.12.1 255.255.255.192     ! Adjust mask if needed
 no shutdown

! Subinterface for VLAN 20
interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 130.34.12.65 255.255.255.192    ! Adjust mask if needed
 no shutdown

end
write memory

### Skills learned
> VLAN creation and naming
> Access port assignment
> 802.1Q trunk configuration between switches
> Router-on-a-Stick inter-VLAN routing
> Multi-switch VLAN design
> Connectivity verification

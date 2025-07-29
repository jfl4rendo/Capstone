# NSA630 Capstone Project Documentation

## Revision Information:
* **Version:** 1.0
* **Date:** 2025-07-28
* **Author:** John Florendo, Ireoluwani Oshinowo, Abdullahi Ajala

## Purpose
This documentation represents our ideas for a small-scale company's network made specifically for our capstone project. Recorded below would detail the physical and and logical layout, network and subnetworks, device details, configurations, and security measures. This documentation could also be used as a reference for troubleshooting, upgrades, and general network management.

## Physical Network Topology

## Logical Network Topology

## Addressing Table
This network uses NAT to translate from our ISP's network of 10.128.250.0/24 to our network of 192.168.10.0/24.

### Network Addressing Table

| Device           | Interface            | IP Address        | Subnet Mask     | Default Gateway   | Role/Notes                                      |
| :--------------- | :------------------- | :---------------- | :-------------- | :---------------- | :---------------------------------------------- |
| **Gateway-Router** | GigabitEthernet0/0/1 | 10.128.209.2      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.1.165     | 255.255.255.224 | N/A (Router)      | Management VLAN (VLAN 10) - HSRP Active         |
|                  | GigabitEthernet0/0.20| 192.168.1.133     | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Active                 |
|                  | GigabitEthernet0/0.30| 192.168.1.5       | 255.255.255.240 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Active          |
|                  | GigabitEthernet0/0.50| 192.168.1.69      | 255.255.255.240 | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Active           |
| **Backup-Router**| GigabitEthernet0/0/1 | 10.128.209.3      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.1.167     | 255.255.255.224 | N/A (Router)      | Management VLAN (VLAN 10) - HSRP Standby        |
|                  | GigabitEthernet0/0.20| 192.168.1.135     | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Standby                |
|                  | GigabitEthernet0/0.30| 192.168.1.7       | 255.255.255.240 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Standby         |
|                  | GigabitEthernet0/0.50| 192.168.1.71      | 255.255.255.240 | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Standby          |
| **HSRP Virtual IPs** |                  |                   |                 |                   | (These are the Default Gateways for Clients)    |
|                  | VLAN 10 (Mgmt)     | 192.168.1.166     | 255.255.255.224 | N/A (VIP)         | Clients in Management VLAN use this as DG       |
|                  | VLAN 20 (IT)       | 192.168.1.134     | 255.255.255.224 | N/A (VIP)         | Clients in IT VLAN use this as DG               |
|                  | VLAN 30 (Employees)| 192.168.1.6       | 255.255.255.240 | N/A (VIP)         | Clients in Employees VLAN use this as DG        |
|                  | VLAN 50 (Wireless) | 192.168.1.70      | 255.255.255.240 | N/A (VIP)         | Clients in Wireless VLAN use this as DG         |
| **Switch-1** | Vlan 10            | 192.168.1.169     | 255.255.255.224 | 192.168.1.166     | Switch Management IP (for remote access/ping)   |
| **Switch-2** | Vlan 10            | 192.168.1.170     | 255.255.255.224 | 192.168.1.166     | Switch Management IP (for remote access/ping)   |
| **ISP Router** | Assumed            | 10.128.209.1      | 255.255.255.0   | N/A               | Your internet gateway                           |
| **End Devices** |                  |                   |                 |                   |                                                 |
| Management PC    | Ethernet           | 192.168.1.168     | 255.255.255.224 | 192.168.1.166     | Example PC in Management VLAN                   |
| IT PC/Server     | Ethernet           | 192.168.1.136     | 255.255.255.224 | 192.168.1.134     | Example PC/Server in IT VLAN                    |
| Employees PC     | Ethernet           | 192.168.1.8       | 255.255.255.240 | 192.168.1.6       | Example PC in Employees VLAN                    |
| Wireless Access Point | Ethernet/Wireless  | 192.168.1.71      | 255.255.255.240 | 192.168.1.70      | Example Wireless Access Point in Wireless VLAN     |
## Running-Configs
These are the running-configs for the devices used:

### Gateway Router Configuration

### Backup Router Configuration

### Switch 1 Configuration

### Switch 2 Configuration 
version 15.0  
no service pad  
service timestamps debug datetime msec  
service timestamps log datetime msec  
no service password-encryption  
hostname Switch-2  
boot-start-marker  
boot-end-marker  
no aaa new-model
system mtu routing 1500  
spanning-tree mode pvst  
spanning-tree extend system-id  
vlan internal allocation policy ascending  
interface Port-channel1  
interface FastEthernet0/1  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/2  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/3  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/4  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/5  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/6  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/7  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/8  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/9  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/10  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/11  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/12  
 switchport access vlan 20  
 switchport mode access  
interface FastEthernet0/13  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/14  
 switchport access vlan 50  
 switchport mode access  
 interface FastEthernet0/15  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/16  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/17  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/18  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/19  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/20  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/21  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/22  
 switchport access vlan 50  
 switchport mode access  
interface FastEthernet0/23  
 switchport mode trunk  
 channel-group 1 mode active  
interface FastEthernet0/24  
 switchport mode trunk  
 channel-group 1 mode active  
interface GigabitEthernet0/1  
 switchport mode trunk  
interface GigabitEthernet0/2  
interface Vlan1  
 no ip address  
 shutdown  
interface Vlan10  
 ip address 192.168.1.170 255.255.255.224  
ip default-gateway 192.168.1.166  
ip http server  
ip http secure-server  
line con 0  
line vty 5 15  
## Server and Service Configurations

## Passwords Management

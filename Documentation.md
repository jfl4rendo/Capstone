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
| **Gateway-Router** | GigabitEthernet0/1   | 10.128.209.2      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.0.1       | 255.255.255.224 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Active      |
|                  | GigabitEthernet0/0.20| 192.168.0.33      | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Active                 |
|                  | GigabitEthernet0/0.30| 192.168.0.65      | 255.255.255.224 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Active          |
|                  | GigabitEthernet0/0.40| 192.168.0.97      | 255.255.255.224 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Active            |
|                  | GigabitEthernet0/0.50| 192.168.1.1       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Active           |
| **Backup-Router**| GigabitEthernet0/1   | 10.128.209.3      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.0.3       | 255.255.255.224 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Standby     |
|                  | GigabitEthernet0/0.20| 192.168.0.35      | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Standby                |
|                  | GigabitEthernet0/0.30| 192.168.0.67      | 255.255.255.224 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Standby         |
|                  | GigabitEthernet0/0.40| 192.168.0.99      | 255.255.255.224 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Standby           |
|                  | GigabitEthernet0/0.50| 192.168.1.3       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Standby          |
| **HSRP Virtual IPs** |                  |                   |                 |                   | (These are the Default Gateways for Clients)    |
|                  | VLAN 10 (HR/Mgmt)  | 192.168.0.2       | 255.255.255.224 | N/A (VIP)         | Clients in HR/Management VLAN use this as DG    |
|                  | VLAN 20 (IT)       | 192.168.0.34      | 255.255.255.224 | N/A (VIP)         | Clients in IT VLAN use this as DG               |
|                  | VLAN 30 (Employees)| 192.168.0.66      | 255.255.255.224 | N/A (VIP)         | Clients in Employees VLAN use this as DG        |
|                  | VLAN 40 (Servers)  | 192.168.0.98      | 255.255.255.224 | N/A (VIP)         | Servers in Servers VLAN use this as DG          |
|                  | VLAN 50 (Wireless) | 192.168.1.2       | 255.255.255.0   | N/A (VIP)         | Clients in Wireless VLAN use this as DG         |
| **Switch-1** | VLAN 20            | 192.168.0.37       | 255.255.255.224 | 192.168.0.34       | Switch Management IP                            |
| **Switch-2** | VLAN 20            | 192.168.0.38       | 255.255.255.224 | 192.168.0.34       | Switch Management IP                            |
| **ISP Router** | Assumed            | 10.128.209.1      | 255.255.255.0   | N/A               | Your internet gateway                           |
| **End Devices (Examples)** |                    |                   |                 |                   | (DHCP configured, these would be assigned)      |
| Management PC    | Ethernet           | 192.168.0.10      | 255.255.255.224 | 192.168.0.2       | Example PC in HR/Management VLAN                |
| IT PC/Server     | Ethernet           | 192.168.0.40      | 255.255.255.224 | 192.168.0.34      | Example PC/Server in IT VLAN                    |
| Employees PC     | Ethernet           | 192.168.0.70      | 255.255.255.224 | 192.168.0.66      | Example PC in Employees VLAN                    |
| AD DS/DNS/DHCP Server   | Ethernet           | 192.168.0.100     | 255.255.255.224 | 192.168.0.98      | Example Server in Servers VLAN                  |
| Email/Web Service Server   | Ethernet           | 192.168.0.101     | 255.255.255.224 | 192.168.0.98      | Example Server in Servers VLAN                  |
| Backup Server   | Ethernet           | 192.168.0.102     | 255.255.255.224 | 192.168.0.98      | Example Server in Servers VLAN                  |
| Server Example   | Ethernet           | 192.168.0.103+     | 255.255.255.224 | 192.168.0.98      | Example Server in Servers VLAN                  |
| Wireless Access Point | Ethernet/Wireless  | 192.168.1.10      | 255.255.255.0   | 192.168.1.2       | Example Wireless device/AP in Wireless VLAN     |
## Running-Configs
These are the running-configs for the devices used:

### Gateway Router Configuration

### Backup Router Configuration

### Switch 1 Configuration

### Switch 2 Configuration 

## Server and Service Configurations

## Passwords Management

 
**DNS Server/DHCP Server/Active Directory Domain Services IP:** 192.168.0.100  
**Email Server/Web Service IP:** 192.168.0.101  
**Backup Server IP:** 192.168.0.102  

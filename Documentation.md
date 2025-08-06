# NSA630 Capstone Project Documentation

## Revision Information:
* **Version:** 1.0
* **Date:** 2025-07-28
* **Author:** John Florendo, Ireoluwani Oshinowo, Abdullahi Ajala

## Purpose
This documentation represents our ideas for a small-scale company's network made specifically for our capstone project. Recorded below would detail the physical and and logical layout, network and subnetworks, device details, configurations, and security measures. This documentation could also be used as a reference for troubleshooting, upgrades, and general network management.

## Physical Network Topology
<img width="2254" height="1351" alt="Office Floor Plan (3)" src="https://github.com/user-attachments/assets/98cadee3-954e-4c67-9919-f867000e5d02" />

## Logical Network Topology
<img width="4560" height="2800" alt="Network Topology (5)" src="https://github.com/user-attachments/assets/710a5bce-f620-48c0-afc2-34dd39b55b76" />

## Addressing Table
This network uses NAT to translate from our ISP's network of 10.128.209.0/24 to our network of 192.168.0.0/23.

### Network Addressing Table

| Device           | Interface            | IP Address        | Subnet Mask     | Default Gateway   | Role/Notes                                      |
| :--------------- | :------------------- | :---------------- | :-------------- | :---------------- | :---------------------------------------------- |
|                  |                      |                   |                 |                   |                                                 |
| **ISP Router** | GigabitEthernet0/1            | 10.128.209.1      | 255.255.255.0   | N/A               | Your internet gateway                           |
|                       |                                     |                                  |                                                               |
| **Gateway-Router** | GigabitEthernet0/1   | 10.128.209.2      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.0.1       | 255.255.255.224 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Active      |
|                  | GigabitEthernet0/0.20| 192.168.0.33      | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Active                 |
|                  | GigabitEthernet0/0.30| 192.168.0.65      | 255.255.255.224 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Active          |
|                  | GigabitEthernet0/0.40| 192.168.0.97      | 255.255.255.224 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Active            |
|                  | GigabitEthernet0/0.50| 192.168.1.1       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Active           |
|                       |                                     |                                  |                                                               |
| **Backup-Router**| GigabitEthernet0/1   | 10.128.209.3      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.0.3       | 255.255.255.224 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Standby     |
|                  | GigabitEthernet0/0.20| 192.168.0.35      | 255.255.255.224 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Standby                |
|                  | GigabitEthernet0/0.30| 192.168.0.67      | 255.255.255.224 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Standby         |
|                  | GigabitEthernet0/0.40| 192.168.0.99      | 255.255.255.224 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Standby           |
|                  | GigabitEthernet0/0.50| 192.168.1.3       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Standby          |
|                       |                                     |                                  |                                                               |
| **HSRP Virtual IPs** |                  |                   |                 |                   | (These are the Default Gateways for Clients)    |
|                  | VLAN 10 (HR/Mgmt)  | 192.168.0.2       | 255.255.255.224 | N/A (VIP)         | Clients in HR/Management VLAN use this as DG    |
|                  | VLAN 20 (IT)       | 192.168.0.34      | 255.255.255.224 | N/A (VIP)         | Clients in IT VLAN use this as DG               |
|                  | VLAN 30 (Employees)| 192.168.0.66      | 255.255.255.224 | N/A (VIP)         | Clients in Employees VLAN use this as DG        |
|                  | VLAN 40 (Servers)  | 192.168.0.98      | 255.255.255.224 | N/A (VIP)         | Servers in Servers VLAN use this as DG          |
|                  | VLAN 50 (Wireless) | 192.168.1.2       | 255.255.255.0   | N/A (VIP)         | Clients in Wireless VLAN use this as DG         |
|                       |                                     |                                  |                                                               |
| **Switch-1** | VLAN 20            | 192.168.0.37       | 255.255.255.224 | 192.168.0.34       | Switch IP on IT VLAN                            |
| **Switch-2** | VLAN 20            | 192.168.0.38       | 255.255.255.224 | 192.168.0.34       | Switch IP on IT VLAN                            |
|                       |                                     |                                  |                                                               |
| **End Devices (Examples)** |                    |                   |                 |                   | (DHCP configured, these would be assigned)      |
| Management PC    | Ethernet           | 192.168.0.10      | 255.255.255.224 | 192.168.0.2       | Example PC in HR/Management VLAN                |
| IT PC     | Ethernet           | 192.168.0.40      | 255.255.255.224 | 192.168.0.34      | Example PC in IT VLAN                    |
| Employees PC     | Ethernet           | 192.168.0.70      | 255.255.255.224 | 192.168.0.66      | Example PC in Employees VLAN                    |
| AD DS/DNS/DHCP Server   | Ethernet           | 192.168.0.101     | 255.255.255.224 | 192.168.0.98      |  AD DS/DNS/DHCP Server in Servers VLAN                  |
| File/Backup Server   | Ethernet           | 192.168.0.102     | 255.255.255.224 | 192.168.0.98      | File/Backup Server in Servers VLAN                  |
| Email/Web Service Server   | Ethernet           | 192.168.0.103     | 255.255.255.224 | 192.168.0.98      | Email/Web Service Server in Servers VLAN                  |
| Server Example   | Ethernet           | 192.168.0.104+     | 255.255.255.224 | 192.168.0.98      | Example Server in Servers VLAN                  |
| Wireless Access Point | Ethernet/Wireless  | 192.168.1.10      | 255.255.255.0   | 192.168.1.2       | Wireless AP in Wireless VLAN     |
## Running-Configs
These are the configurations for the routers and switches that were used:

### Gateway Router Configuration

### Backup Router Configuration

### Switch 1 Configuration

### Switch 2 Configuration 

## Server and Service Configurations
* Active Directory Domain Services  
* DNS Server  
* DHCP Server  
* File Server  
* Email Server  

## Passwords Management

When it comes to password management, we would be using Bitwarden, Microsoft Entra ID, and Microsoft Sentinel to create a robust and secure system. Their synergy focuses on centralizing identities, enforcing strong policies, and actively monitoring for threats.

### Centralized Identity and Policy Enforcement

* **Microsoft Entra ID** is the foundation, acting as the primary identity provider for your organization. It ensures that every user has a unique, centrally managed account. You can enforce your organization's security policies, such as Multi-Factor Authentication (MFA) and conditional access rules, directly within Entra ID.
* **Bitwarden** integrates with Entra ID through Single Sign-On (SSO). This means employees use their Entra ID credentials and MFA to access their secure Bitwarden vaults. You don't have to manage a separate set of identities for the password manager itself. This also means if a user leaves the company and their Entra ID account is disabled, they automatically lose access to their Bitwarden vault.

### Threat Monitoring and Automated Response

* **Microsoft Sentinel** is the security nerve center. It pulls in logs from both Bitwarden and Entra ID to monitor for suspicious activity.
* **Entra ID's logs** provide Sentinel with a stream of authentication events, allowing it to detect threats like unusual login locations, multiple failed sign-in attempts, or attempts to bypass MFA.
* **Bitwarden's logs** add a critical layer of visibility by showing how users are interacting with their stored passwords. For example, Sentinel can be configured to alert if a user who logged in from an unusual location suddenly accesses a large number of sensitive credentials in a short period. This correlation is a powerful way to identify a compromised account.
* If Sentinel detects a high-risk event, it can trigger an automated response. For instance, a playbook could be configured to immediately disable a user's Entra ID account and force a password reset for their Bitwarden vault, effectively neutralizing the threat.

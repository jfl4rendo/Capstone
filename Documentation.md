# NSA630 Capstone Project Documentation

## Revision Information:
* **Version:** 1.0
* **Date:** 2025-07-28
* **Author:** John Florendo, Ireoluwani Oshinowo, Abdullahi Ajala

## Purpose
This documentation represents our ideas for a small-scale company's network made specifically for our capstone project. Recorded below would detail the physical and and logical layout, network and subnetworks, device details, configurations, and security measures. This documentation could also be used as a reference for troubleshooting, upgrades, and general network management.

## Physical Network Topology
<img width="2254" height="1351" alt="Office Floor Plan" src="https://github.com/user-attachments/assets/ebe99bf3-f15e-4284-9f60-b06ebd1cd78c" />

## Logical Network Topology
<img width="4560" height="2800" alt="Network Topology" src="https://github.com/user-attachments/assets/7497518f-f8a3-436b-963a-a32dda851ae4" />

## Addressing Table
This network uses NAT to translate from our ISP's network of 10.128.209.0/24 to our network of 192.168.0.0/23.

### Network Addressing Table

| Device           | Interface            | IP Address        | Subnet Mask     | Default Gateway   | Role/Notes                                      |
| :--------------- | :------------------- | :---------------- | :-------------- | :---------------- | :---------------------------------------------- |
|                  |                      |                   |                 |                   |                                                 |
| **ISP Firewall** | GigabitEthernet0/1            | 10.128.209.x (DHCP)      | 255.255.255.0   | N/A               | Connection to ISP                 |
|                  | GigabitEthernet0/1            | 192.168.80.1      | 255.255.255.0   | N/A               |Internet gateway                     |
|                       |                                     |                                  |                                                               |
| **Gateway-Router** | GigabitEthernet0/1   | 192.168.80.2      | 255.255.255.0   | N/A      | Connection to ISP Firewall                |
|                  | GigabitEthernet0/0.10| 192.168.10.1       | 255.255.255.0 | N/A (Router)      | HR VLAN (VLAN 10) - HSRP Active      |
|                  | GigabitEthernet0/0.20| 192.168.20.1      | 255.255.255.0 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Active                 |
|                  | GigabitEthernet0/0.30| 192.168.30.1      | 255.255.255.0 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Active          |
|                  | GigabitEthernet0/0.40| 192.168.40.1      | 255.255.255.0 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Active            |
|                  | GigabitEthernet0/0.50| 192.168.50.1       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Active           |
|                  | GigabitEthernet0/0.99| 192.168.99.1       | 255.255.255.0   | N/A (Router)      | Native VLAN (VLAN 99) - HSRP Active           |
|                  | GigabitEthernet0/0.100| 192.168.100.1       | 255.255.255.0   | N/A (Router)      | Management VLAN (VLAN 100) - HSRP Active           |
|                       |                                     |                                  |                                                               |
| **Backup-Router**| GigabitEthernet0/1   | 192.168.80.3      | 255.255.255.0   | N/A      | Connection to ISP Firewall                |
|                  | GigabitEthernet0/0.10| 192.168.10.2       | 255.255.255.0 | N/A (Router)      | HR VLAN (VLAN 10) - HSRP Standby     |
|                  | GigabitEthernet0/0.20| 192.168.20.2      | 255.255.255.0 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Standby                |
|                  | GigabitEthernet0/0.30| 192.168.30.2      | 255.255.255.0 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Standby         |
|                  | GigabitEthernet0/0.40| 192.168.40.2      | 255.255.255.0 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Standby           |
|                  | GigabitEthernet0/0.50| 192.168.50.2       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Standby          |
|                  | GigabitEthernet0/0.99| 192.168.99.2       | 255.255.255.0   | N/A (Router)      | Native VLAN (VLAN 99) - HSRP Active           |
|                  | GigabitEthernet0/0.100| 192.168.100.2       | 255.255.255.0   | N/A (Router)      | Management VLAN (VLAN 100) - HSRP Active           |
|                       |                                     |                                  |                                                               |
| **HSRP Virtual IPs** |                  |                   |                 |                   | (Default Gateways)    |
|                  | VLAN 10 (HR/Mgmt)  | 192.168.10.11       | 255.255.255.0 | N/A (Virtual IP)         | HR VLAN Default Gateway    |
|                  | VLAN 20 (IT)       | 192.168.20.11      | 255.255.255.0 | N/A (Virtual IP)         | IT VLAN Default Gateway               |
|                  | VLAN 30 (Employees)| 192.168.30.11      | 255.255.255.0 | N/A (Virtual IP)         | Employees VLAN Default Gateway        |
|                  | VLAN 40 (Servers)  | 192.168.40.11      | 255.255.255.0 | N/A (Virtual IP)         | Servers VLAN Default Gateway          |
|                  | VLAN 50 (Wireless) | 192.168.50.11       | 255.255.255.0   | N/A (Virtual IP)         | Wireless VLAN Default Gateway         |
|                  | VLAN 99 (Native)  | 192.168.99.11      | 255.255.255.0 | N/A (Virtual IP)         | Native VLAN Default Gateway          |
|                  | VLAN 100 (Management)  | 192.168.100.11      | 255.255.255.0 | N/A (Virtual IP)         | Management VLAN Default Gateway          |
|                       |                                     |                                  |                                                               |
| **Switch-1** | VLAN 20            | 192.168.100.3       | 255.255.255.0 | 192.168.100.11       | Switch IP on Management VLAN                            |
| **Switch-2** | VLAN 20            | 192.168.100.4       | 255.255.255.0 | 192.168.100.11       | Switch IP on Management VLAN                            |
|                       |                                     |                                  |                                                               |
| **End Devices** |                    |                   |                 |                   | (DHCP configured, these would be assigned)      |
| Management PC    | Ethernet           | 192.168.0.10      | 255.255.255.224 | 192.168.0.2       | Example PC in HR/Management VLAN                |
| IT PC     | Ethernet           | 192.168.0.40      | 255.255.255.224 | 192.168.0.34      | Example PC in IT VLAN                    |
| Employees PC     | Ethernet           | 192.168.0.70      | 255.255.255.224 | 192.168.0.66      | Example PC in Employees VLAN                    |
| AD DS/DNS/DHCP Server   | Ethernet           | 192.168.0.101     | 255.255.255.224 | 192.168.0.98      |  AD DS/DNS/DHCP Server in Servers VLAN                  |
| File/Backup Server   | Ethernet           | 192.168.0.102     | 255.255.255.224 | 192.168.0.98      | File/Backup Server in Servers VLAN                  |
| Email/Web Service Server   | Ethernet           | 192.168.0.103     | 255.255.255.224 | 192.168.0.98      | Email/Web Service Server in Servers VLAN                  |
| Wireless Access Point | Ethernet/Wireless  | 192.168.1.10      | 255.255.255.0   | 192.168.1.2       | Wireless AP in Wireless VLAN     |
## Running-Configs
These are the configurations for the routers and switches that were used:

### Gateway Router Configuration

### Backup Router Configuration

### Switch 1 Configuration

### Switch 2 Configuration 

## Server and Service Configurations
### Active Directory Domain Services
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d2afc0f0-a459-4400-8b3e-a9d9a514449d" />
<div align="center">The Two Domain Controllers</div>  
  

### DNS Server
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ea0ec2e7-2566-4571-87a1-f4139fa190a5" />
<div align="center">DNS Server on DC1</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a24e9e63-6668-44de-92a5-3938f5e413e1" />
<div align="center">DNS Server on DC2</div>  
  

### DHCP Server
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0d7c0771-00c5-4417-935c-4128fbf506a8" />
<div align="center">DC1 HR VLAN DHCP Pool</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/278a0e01-8628-41dc-adc9-0626630f7132" />
<div align="center">DC1 IT VLAN DHCP Pool</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad04325b-5566-4ad2-b4b2-d7eae0672d6f" />
<div align="center">DC1 Employees VLAN DHCP Pool</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fa9654e-e842-40b2-8e28-e27926ec2dc4" />
<div align="center">DC2 HR VLAN DHCP Pool (Failover)</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c810bc92-5de2-4ff1-9abe-8b320fdada1d" />
<div align="center">DC2 IT VLAN DHCP Pool (Failover)</div>  
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4010fd7c-5e10-43ac-800a-56e3f6f28cad" />
<div align="center">DC2 Employees VLAN DHCP Pool (Failover)</div>  
  

### File Server
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d62ce921-7587-4a7a-a65c-5fb018c80f1c" />
<div align="center">Shared Folder(N:) found in File Share Server</div>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7157ad31-edcf-4f2e-ab1a-951f13df8c38" />
<div align="center">Shared Folder(N:) being accessed in the Domain Controller</div>

### Email Server  
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/4a337836-644c-4092-b875-da7dfcb711c9" />
<div align="center">All the Users from the Active Directory found in our Office 365 directory</div>

### Additional Work
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3151177a-bae4-4ebe-9eab-c3f70d297960" />
<div align="center">Network Scan</div>

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

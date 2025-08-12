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
<img width="4560" height="2800" alt="Network Topology" src="https://github.com/user-attachments/assets/d3c3e359-0213-436b-b6e2-d186dc9aac86" />

## Addressing Table
This network uses NAT to translate from our ISP's network of 10.128.209.0/24 to our network of 192.168.0.0/23.

### Network Addressing Table

| Device           | Interface            | IP Address        | Subnet Mask     | Default Gateway   | Role/Notes                                      |
| :--------------- | :------------------- | :---------------- | :-------------- | :---------------- | :---------------------------------------------- |
|                  |                      |                   |                 |                   |                                                 |
| **ISP Router** | GigabitEthernet0/1            | 10.128.209.1      | 255.255.255.0   | N/A               | Your internet gateway                           |
|                       |                                     |                                  |                                                               |
| **Gateway-Router** | GigabitEthernet0/1   | 10.128.209.2      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.10.1       | 255.255.255.0 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Active      |
|                  | GigabitEthernet0/0.20| 192.168.20.1      | 255.255.255.0 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Active                 |
|                  | GigabitEthernet0/0.30| 192.168.30.1      | 255.255.255.0 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Active          |
|                  | GigabitEthernet0/0.40| 192.168.40.1      | 255.255.255.0 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Active            |
|                  | GigabitEthernet0/0.50| 192.168.50.1       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Active           |
|                       |                                     |                                  |                                                               |
| **Backup-Router**| GigabitEthernet0/1   | 10.128.209.3      | 255.255.255.0   | 10.128.209.1      | Connection to ISP (Outside NAT)                 |
|                  | GigabitEthernet0/0.10| 192.168.10.2       | 255.255.255.0 | N/A (Router)      | HR/Management VLAN (VLAN 10) - HSRP Standby     |
|                  | GigabitEthernet0/0.20| 192.168.20.2      | 255.255.255.0 | N/A (Router)      | IT VLAN (VLAN 20) - HSRP Standby                |
|                  | GigabitEthernet0/0.30| 192.168.30.2      | 255.255.255.0 | N/A (Router)      | Employees VLAN (VLAN 30) - HSRP Standby         |
|                  | GigabitEthernet0/0.40| 192.168.40.2      | 255.255.255.0 | N/A (Router)      | Servers VLAN (VLAN 40) - HSRP Standby           |
|                  | GigabitEthernet0/0.50| 192.168.50.2       | 255.255.255.0   | N/A (Router)      | Wireless VLAN (VLAN 50) - HSRP Standby          |
|                       |                                     |                                  |                                                               |
| **HSRP Virtual IPs** |                  |                   |                 |                   | (These are the Default Gateways for Clients)    |
|                  | VLAN 10 (HR/Mgmt)  | 192.168.10.3       | 255.255.255.0 | N/A (VIP)         | Clients in HR/Management VLAN use this as DG    |
|                  | VLAN 20 (IT)       | 192.168.20.3      | 255.255.255.0 | N/A (VIP)         | Clients in IT VLAN use this as DG               |
|                  | VLAN 30 (Employees)| 192.168.30.3      | 255.255.255.0 | N/A (VIP)         | Clients in Employees VLAN use this as DG        |
|                  | VLAN 40 (Servers)  | 192.168.40.3      | 255.255.255.0 | N/A (VIP)         | Servers in Servers VLAN use this as DG          |
|                  | VLAN 50 (Wireless) | 192.168.50.3       | 255.255.255.0   | N/A (VIP)         | Clients in Wireless VLAN use this as DG         |
|                       |                                     |                                  |                                                               |
| **Switch-1** | VLAN 20            | 192.168.0.37       | 255.255.255.224 | 192.168.0.34       | Switch IP on IT VLAN                            |
| **Switch-2** | VLAN 20            | 192.168.0.38       | 255.255.255.224 | 192.168.0.34       | Switch IP on IT VLAN                            |
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
version 15.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname Switch-1
!
boot-start-marker
boot-end-marker
!
!
username admin privilege 15 secret 5 $1$R5XH$dAXvEZ9HjoCoTwBQdwJXF1
no aaa new-model
system mtu routing 1500
!
!
!
!
!
!
!
login block-for 120 attempts 5 within 30
!
!
crypto pki trustpoint TP-self-signed-3211258496
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-3211258496
 revocation-check none
 rsakeypair TP-self-signed-3211258496
!
!
crypto pki certificate chain TP-self-signed-3211258496
 certificate self-signed 01
  3082022B 30820194 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 33323131 32353834 3936301E 170D3933 30333031 30303030
  35375A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D33 32313132
  35383439 3630819F 300D0609 2A864886 F70D0101 01050003 818D0030 81890281
  8100A15A 3D4DB1D3 897F77BD 6F09417A 1D479E3A C5CDB2EF 9F524847 010CFD04
  0B09192F 170FAEC6 9C18E388 9CA430CB B3C2BA5C F74811C1 B71A06FA 75B7497F
  63B710F5 30199055 964907E0 579D7B3D 17C6C825 024F1EE9 1562F1C9 07E72460
  CE52C65C 8E85A897 59952666 124D8BA6 B51D1DBB D4231197 FEDE6D6B 6B5BB1A0
  24A70203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 301F0603
  551D2304 18301680 147FCD21 7AA9CFF4 911D76DB 21C74310 FFDB5F1A 62301D06
  03551D0E 04160414 7FCD217A A9CFF491 1D76DB21 C74310FF DB5F1A62 300D0609
  2A864886 F70D0101 05050003 8181007C A5386F02 D3014622 2660AB5D BF6C87E2
  CC183138 A8206EA4 014141FB 2076A363 66E3C79F 59E4AEF5 7416D2A5 3C3B1094
  4CFB5BD0 0F34BFC5 B18603F3 1C711DC2 B03AE39C F9EA12C5 C218465B CF5B4C0F
  3054243D 2B86D43D 3E81FFA4 AB4D082D DC570E6F 24238614 D4B3BC67 622C8E43
  D48DB38E C5471E7C 747B2026 C7A958
        quit
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
interface Port-channel1
 switchport mode trunk
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/4
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/5
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/6
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/7
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/8
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/9
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/10
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/11
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/12
 switchport access vlan 10
 switchport mode access
 no cdp enable
!
interface FastEthernet0/13
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/14
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/15
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/16
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/17
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/18
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/19
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/20
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/21
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/22
 switchport access vlan 30
 switchport mode access
 no cdp enable
!
interface FastEthernet0/23
 switchport mode trunk
 no cdp enable
 channel-group 1 mode active
!
interface FastEthernet0/24
 switchport mode trunk
 no cdp enable
 channel-group 1 mode active
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan20
 ip address 192.168.0.37 255.255.255.224
!
ip default-gateway 192.168.0.34
ip http server
ip http secure-server
!
no vstack
!
line con 0
 exec-timeout 5 0
 password 7 00341242404C5B140B
 logging synchronous
 login local
 transport preferred none
 stopbits 1
line vty 0 4
 exec-timeout 5 0
 login local
 transport input ssh
line vty 5 15
 exec-timeout 5 0
 login local
 transport input ssh
!
end
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

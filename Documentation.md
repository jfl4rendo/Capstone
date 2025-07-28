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

| Network | IP Address | Notes | 
|----------|----------|----------|
| Gateway Router | 192.168.10.1 | |
| Backup Router | 192.168.10.2 | |
| Management | | |
| Employees | | |
| Servers | | |
| Guest Wi-Fi | | |
## Running-Configs

## Server and Service Configurations

## Passwords Management

VLAN 1
VLAN 10 Management
Vlan 50 Employees
Vlan 80 Guests
Vlan 99 IT
Vlan 999 Native

Switch 1:
Ports 1-12: Management
Ports 13-22: Employees
Ports 23-24: EtherChannel

Switch 2:
Ports 1-12: Guest Wi-Fi
Ports 13-22: Servers
Ports 23-24: EtherChannel

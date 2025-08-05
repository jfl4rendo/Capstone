Gateway-Router

// --- ISP Interface ---
interface GigabitEthernet0/1
ip address 10.128.209.2 255.255.255.0
ip nat outside
no shutdown

// --- Interface to Switch ---
interface GigabitEthernet0/0
no ip address
no shutdown

// --- Sub-interfaces for VLANs (Internal Gateways with HSRP) ---

// HR/Management VLAN (VLAN 10) - 192.168.0.0/27
interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.0.1 255.255.255.224
ip nat inside
standby 10 ip 192.168.0.2
standby 10 priority 110
standby 10 preempt

// IT VLAN (VLAN 20) - 192.168.0.32/27
interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.0.33 255.255.255.224
ip nat inside
standby 20 ip 192.168.0.34
standby 20 priority 110
standby 20 preempt

// Employees VLAN (VLAN 30) - 192.168.0.64/27
interface GigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.0.65 255.255.255.224
ip nat inside
standby 30 ip 192.168.0.66
standby 30 priority 110
standby 30 preempt

// Servers VLAN (VLAN 40) - 192.168.0.96/27
interface GigabitEthernet0/0.40
encapsulation dot1Q 40
ip address 192.168.0.97 255.255.255.224
ip nat inside
standby 40 ip 192.168.0.98
standby 40 priority 110
standby 40 preempt

// Wireless VLAN (VLAN 50) - 192.168.1.0/24
interface GigabitEthernet0/0.50
encapsulation dot1Q 50
ip address 192.168.1.1 255.255.255.0
ip nat inside
standby 50 ip 192.168.1.2
standby 50 priority 110
standby 50 preempt

// --- OSPF Configuration ---
router ospf 1
router-id 1.1.1.1
network 192.168.0.0 0.0.1.255 area 0 // Covers 192.168.0.0/23
network 10.128.209.0 0.0.0.255 area 0

// --- NAT Configuration ---
access-list 1 permit 192.168.0.0 0.0.1.255 // Covers 192.168.0.0/23
ip nat inside source list 1 interface GigabitEthernet0/1 overload

// --- Default Route to ISP ---
ip route 0.0.0.0 0.0.0.0 10.128.209.1

Backup-Router

// --- ISP Interface ---
interface GigabitEthernet0/1
ip address 10.128.209.3 255.255.255.0
ip nat outside
no shutdown

// --- Interface to Switch ---
interface GigabitEthernet0/0
no ip address
no shutdown

// --- Sub-interfaces for VLANs (Internal Gateways with HSRP) ---

// HR/Management VLAN (VLAN 10) - 192.168.0.0/27
interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.0.3 255.255.255.224
ip nat inside
standby 10 ip 192.168.0.2
standby 10 preempt

// IT VLAN (VLAN 20) - 192.168.0.32/27
interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.0.35 255.255.255.224
ip nat inside
standby 20 ip 192.168.0.34
standby 20 preempt

// Employees VLAN (VLAN 30) - 192.168.0.64/27
interface GigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.0.67 255.255.255.224
ip nat inside
standby 30 ip 192.168.0.66
standby 30 preempt

// Servers VLAN (VLAN 40) - 192.168.0.96/27
interface GigabitEthernet0/0.40
encapsulation dot1Q 40
ip address 192.168.0.99 255.255.255.224
ip nat inside
standby 40 ip 192.168.0.98
standby 40 preempt

// Wireless VLAN (VLAN 50) - 192.168.1.0/24
interface GigabitEthernet0/0.50
encapsulation dot1Q 50
ip address 192.168.1.3 255.255.255.0
ip nat inside
standby 50 ip 192.168.1.2
standby 50 preempt

// --- OSPF Configuration ---
router ospf 1
router-id 2.2.2.2
network 192.168.0.0 0.0.1.255 area 0 // Covers 192.168.0.0/23
network 10.128.209.0 0.0.0.255 area 0

// --- NAT Configuration ---
access-list 1 permit 192.168.0.0 0.0.1.255 // Covers 192.168.0.0/23
ip nat inside source list 1 interface GigabitEthernet0/1 overload

// --- Default Route to ISP ---
ip route 0.0.0.0 0.0.0.0 10.128.209.1

Switch-1

// --- VLAN Definitions ---
vlan 10
name HR_Management
vlan 20
name IT
vlan 30
name Employees
vlan 40
name Servers
vlan 50
name Wireless

// --- Trunk Port for Gateway Router Connection (G0/1) ---
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

// --- Inter-Switch Trunk (EtherChannel to Switch-2) ---
interface Port-channel 1
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

interface range FastEthernet0/23, FastEthernet0/24
channel-group 1 mode active
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

// --- Configure Access Ports for End Devices ---

// Ports 1-12 for HR/Management (VLAN 10)
interface range FastEthernet0/1 - 12
switchport mode access
switchport access vlan 10
no shutdown

// Ports 13-22 for Employees (VLAN 30)
interface range FastEthernet0/13 - 22
switchport mode access
switchport access vlan 30
no shutdown

// --- Switch Management IP and Default Gateway ---
no interface vlan 10 // Remove previous management IP
interface vlan 20
ip address 192.168.0.37 255.255.255.224
no shutdown
exit
ip default-gateway 192.168.0.34

Switch-2

// --- VLAN Definitions ---
vlan 10
name HR_Management
vlan 20
name IT
vlan 30
name Employees
vlan 40
name Servers
vlan 50
name Wireless

// --- Trunk Port for Backup Router Connection (G0/1) ---
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

// --- Inter-Switch Trunk (EtherChannel to Switch-1) ---
interface Port-channel 1
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

interface range FastEthernet0/23, FastEthernet0/24
channel-group 1 mode active
switchport mode trunk
switchport trunk encapsulation dot1q
no shutdown

// --- Configure Access Ports for End Devices ---

// Ports 1-5 for Servers (VLAN 40)
interface range FastEthernet0/1 - 5
switchport mode access
switchport access vlan 40
no shutdown

// Ports 6-12 for IT (VLAN 20)
interface range FastEthernet0/6 - 12
switchport mode access
switchport access vlan 20
no shutdown

// Ports 13-22 for Wireless (VLAN 50)
interface range FastEthernet0/13 - 22
switchport mode access
switchport access vlan 50
no shutdown

// --- Switch Management IP and Default Gateway ---
no interface vlan 10 // Remove previous management IP
interface vlan 20
ip address 192.168.0.38 255.255.255.224
no shutdown
exit
ip default-gateway 192.168.0.34


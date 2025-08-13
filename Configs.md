Gateway-Router
!
boot-start-marker
boot-end-marker
!
!
logging buffered 512000
enable secret 5 $1$Gpn.$7Pm/JU43Rb5XYLMK6SOf.1
!
no aaa new-model
!
!
!
!
!
!
!
!
!
!
!
!
!
!
ip domain name nexus.local
ip cef
login block-for 120 attempts 5 within 30
no ipv6 cef
!
multilink bundle-name authenticated
!
!
cts logging verbose
!
!
license udi pid CISCO2901/K9 sn FJC2037A0Q4
license boot module c2900 technology-package securityk9
!
!
username admin privilege 15 secret 5 $1$SKb6$96D2wpEZffuRA7Nj9bj4V.
!
redundancy
!
!
!
!
no cdp run
!
ip ssh version 2
!
!
!
!
!
!
!
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 10 ip 192.168.10.11
 no cdp enable
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 20 ip 192.168.20.11
 no cdp enable
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 ip access-group block_employees in
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 30 ip 192.168.30.11
!
interface GigabitEthernet0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 40 ip 192.168.40.11
!
interface GigabitEthernet0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 ip access-group block_wireless in
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 50 ip 192.168.50.11
!
interface GigabitEthernet0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0
 ip ospf 1 area 0
 vrrp 99 ip 192.168.99.11
!
interface GigabitEthernet0/0.100
 encapsulation dot1Q 100
 ip address 192.168.100.1 255.255.255.0
 ip ospf 1 area 0
 vrrp 100 ip 192.168.100.11
!
interface GigabitEthernet0/1
 ip address 192.168.80.2 255.255.255.0
 ip ospf 1 area 0
 duplex auto
 speed auto
!
interface Serial0/0/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/0/1
 no ip address
 shutdown
 clock rate 2000000
!
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.40.0 0.0.0.255 area 0
 network 192.168.50.0 0.0.0.255 area 0
 network 192.168.80.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 192.168.100.0 0.0.0.255 area 0
 default-information originate always
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 192.168.80.1
!
ip access-list extended block_employees
 deny   ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny   ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny   ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny   ip 192.168.30.0 0.0.0.255 192.168.50.0 0.0.0.255
 deny   tcp any any eq telnet
 deny   tcp any any eq 22
 permit ip 192.168.30.0 0.0.0.255 any
ip access-list extended block_wireless
 deny   ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny   ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny   ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
 deny   ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny   tcp any any eq telnet
 deny   tcp any any eq 22
 permit ip 192.168.50.0 0.0.0.255 any
!
!
!
!
control-plane
!
!
!
line con 0
 exec-timeout 5 0
 password 7 0236051F4F115F3348
 logging synchronous
 login local
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 exec-timeout 5 0
 login local
 transport input ssh
!
scheduler allocate 20000 1000

Backup-Router
!
boot-start-marker
boot-end-marker
!
!
vrf definition Mgmt-intf
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
 exit-address-family
!
logging buffered 512000
enable secret 9 $9$5pQjacKFIAUyJE$4oTT/3.XYhsG7E9d3S.yEHGmlbNObQ6ooH2fgHfkL7E
!
no aaa new-model
call-home
 ! If contact email address in call-home is configured as sch-smart-licensing@cisco.com
 ! the email address configured in Cisco Smart License Portal will be used as contact email address to send SCH notifications.
 contact-email-addr sch-smart-licensing@cisco.com
 profile "CiscoTAC-1"
  active
  destination transport-method http
  no destination transport-method email
!
ip domain name nexus.local
!
!
!
login block-for 120 attempts 5 within 30
login on-success log
!
!
!
!
!
!
!
subscriber templating
!
!
!
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
crypto pki trustpoint TP-self-signed-3197825143
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-3197825143
 revocation-check none
 rsakeypair TP-self-signed-3197825143
!
crypto pki trustpoint SLA-TrustPoint
 enrollment pkcs12
 revocation-check crl
!
!
crypto pki certificate chain TP-self-signed-3197825143
 certificate self-signed 01
  30820330 30820218 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 33313937 38323531 3433301E 170D3235 30383033 31303133
  30365A17 0D333030 31303130 30303030 305A3031 312F302D 06035504 03132649
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D33 31393738
  32353134 33308201 22300D06 092A8648 86F70D01 01010500 0382010F 00308201
  0A028201 0100DFB1 40FDC937 A19CC7ED D49E430A 55244177 CF6B5395 AE473EAD
  9AC8E4E2 52C9E0A6 D8D31C73 B53591CC C7928876 C39F00EB 4635A62B 1A4D5D2B
  F388B218 55E90450 A8FD5390 E187C310 BEF768C8 95E74D4D FAB505FD 454A5724
  AD3AD9AA 9FF0387C F41AFE99 996209A8 16334A79 CADC8811 2155B4D7 4A8B3F2D
  2947E59A B7668A0F 126FC0BD 7BAE4561 07569202 9838B3D4 768459C9 E6C86B2F
  CD143C79 B9131AB7 EA8C625A 9378B0D3 AE85E8B7 73728E81 0680466E B09A89AE
  DC549CCF 280BCCEA EA4BB786 41F833CF 8CB55EC0 0DF92DEF E132478C 7FCE44C5
  540529E4 B3ACF430 79D386C2 5CA401D2 9756730E E40EA4D2 AA471C62 711AA9D1
  7F6E3316 2A4D0203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF
  301F0603 551D2304 18301680 140CE5C2 82592D73 A7EE2085 0ECDA8F0 BDB87403
  3B301D06 03551D0E 04160414 0CE5C282 592D73A7 EE20850E CDA8F0BD B874033B
  300D0609 2A864886 F70D0101 05050003 82010100 031FFF33 3CE13FD6 8BC733D7
  E8954FF4 53FC4C8B B519BD8D E511AC59 B30DB031 5197FC5B CA3D43B7 58AD44D6
  77783CD6 B6944979 AF88F7F8 394E9D1B C4D853F5 01D16B2E DD7F091E BB6E87C3
  8B5CCEE9 098C54C3 EAA7CC31 E53C4D43 B2455EBD 35B3B9B9 84F5D8CC D0A54B59
  3BFA3FBE 78E41095 65C68EA7 D908052A 9B9A00BC 34153E56 E59CA932 035C097D
  10F9C7FD 2B81249F D4FC6573 A37B15E8 DEC29CC9 01D6758E C5F4FBA7 41BE8C76
  6EAC608B 5BC8A0E7 70FF3228 FBB852F2 90D6008A B42809BE 0C8D98AF 53527657
  27E7CB54 F410FBC5 BA4CC147 5825A99C 5D03338D 35AC6704 84CA2B81 7BA50FC7
  9D11475F 2AA4E6FD 43196C3D 9E9B99B8 D8D70FB4
        quit
crypto pki certificate chain SLA-TrustPoint
 certificate ca 01
  30820321 30820209 A0030201 02020101 300D0609 2A864886 F70D0101 0B050030
  32310E30 0C060355 040A1305 43697363 6F312030 1E060355 04031317 43697363
  6F204C69 63656E73 696E6720 526F6F74 20434130 1E170D31 33303533 30313934
  3834375A 170D3338 30353330 31393438 34375A30 32310E30 0C060355 040A1305
  43697363 6F312030 1E060355 04031317 43697363 6F204C69 63656E73 696E6720
  526F6F74 20434130 82012230 0D06092A 864886F7 0D010101 05000382 010F0030
  82010A02 82010100 A6BCBD96 131E05F7 145EA72C 2CD686E6 17222EA1 F1EFF64D
  CBB4C798 212AA147 C655D8D7 9471380D 8711441E 1AAF071A 9CAE6388 8A38E520
  1C394D78 462EF239 C659F715 B98C0A59 5BBB5CBD 0CFEBEA3 700A8BF7 D8F256EE
  4AA4E80D DB6FD1C9 60B1FD18 FFC69C96 6FA68957 A2617DE7 104FDC5F EA2956AC
  7390A3EB 2B5436AD C847A2C5 DAB553EB 69A9A535 58E9F3E3 C0BD23CF 58BD7188
  68E69491 20F320E7 948E71D7 AE3BCC84 F10684C7 4BC8E00F 539BA42B 42C68BB7
  C7479096 B4CB2D62 EA2F505D C7B062A4 6811D95B E8250FC4 5D5D5FB8 8F27D191
  C55F0D76 61F9A4CD 3D992327 A8BB03BD 4E6D7069 7CBADF8B DF5F4368 95135E44
  DFC7C6CF 04DD7FD1 02030100 01A34230 40300E06 03551D0F 0101FF04 04030201
  06300F06 03551D13 0101FF04 05300301 01FF301D 0603551D 0E041604 1449DC85
  4B3D31E5 1B3E6A17 606AF333 3D3B4C73 E8300D06 092A8648 86F70D01 010B0500
  03820101 00507F24 D3932A66 86025D9F E838AE5C 6D4DF6B0 49631C78 240DA905
  604EDCDE FF4FED2B 77FC460E CD636FDB DD44681E 3A5673AB 9093D3B1 6C9E3D8B
  D98987BF E40CBD9E 1AECA0C2 2189BB5C 8FA85686 CD98B646 5575B146 8DFC66A8
  467A3DF4 4D565700 6ADF0F0D CF835015 3C04FF7C 21E878AC 11BA9CD2 55A9232C
  7CA7B7E6 C1AF74F6 152E99B7 B1FCF9BB E973DE7F 5BDDEB86 C71E3B49 1765308B
  5FB0DA06 B92AFE7F 494E8A9E 07B85737 F3A58BE1 1A48A229 C37C1E69 39F08678
  80DDCD16 D6BACECA EEBC7CF9 8428787B 35202CDC 60E4616A B623CDBD 230E3AFB
  418616A9 4093E049 4D10AB75 27E86F73 932E35B5 8862FDAE 0275156F 719BB2F0
  D697DF7F 28
        quit
!
!
!
!
!
!
!
!
!
no license feature hseck9
license udi pid ISR4321/K9 sn FDO221418T8
license accept end user agreement
license boot suite AdvUCSuiteK9
license boot level securityk9
memory free low-watermark processor 67107
!
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
username admin privilege 15 secret 9 $9$KMtVRlgxivq4Mk$YVmyqnQ98sPj0Zc8nZibUix1dJaI/aWCNCYlF.V9/cw
!
redundancy
 mode none
!
!
!
!
!
no cdp run
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface GigabitEthernet0/0/0
 no ip address
 ip ospf 1 area 0
 negotiation auto
!
interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.2 255.255.255.0
 ip helper-address 192.168.40.2
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 no cdp enable
 vrrp 10 ip 192.168.10.11
!
interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.2 255.255.255.0
 ip helper-address 192.168.40.2
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 no cdp enable
 vrrp 20 ip 192.168.20.11
!
interface GigabitEthernet0/0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.2 255.255.255.0
 ip helper-address 192.168.40.101
 ip access-group block_employees in
 ip ospf 1 area 0
 vrrp 30 ip 192.168.30.11
!
interface GigabitEthernet0/0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.2 255.255.255.0
 ip helper-address 192.168.40.101
 ip ospf 1 area 0
 vrrp 40 ip 192.168.40.11
!
interface GigabitEthernet0/0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.2 255.255.255.0
 ip helper-address 192.168.40.101
 ip access-group block_wireless in
 ip ospf 1 area 0
 vrrp 50 ip 192.168.50.11
!
interface GigabitEthernet0/0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.2 255.255.255.0
 ip ospf 1 area 0
 vrrp 99 ip 192.168.99.11
!
interface GigabitEthernet0/0/0.100
 encapsulation dot1Q 100
 ip address 192.168.100.2 255.255.255.0
 ip ospf 1 area 0
 vrrp 100 ip 192.168.100.11
!
interface GigabitEthernet0/0/1
 ip address 192.168.80.3 255.255.255.0
 ip ospf 1 area 0
 negotiation auto
!
interface Serial0/1/0
 no ip address
 shutdown
!
interface Serial0/1/1
 no ip address
 shutdown
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 negotiation auto
!
router ospf 1
 router-id 2.2.2.2
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.40.0 0.0.0.255 area 0
 network 192.168.50.0 0.0.0.255 area 0
 network 192.168.80.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 192.168.100.0 0.0.0.255 area 0
!
ip forward-protocol nd
no ip http server
ip http authentication local
no ip http secure-server
ip http client source-interface GigabitEthernet0/0/1
ip tftp source-interface GigabitEthernet0/0/1
ip route 0.0.0.0 0.0.0.0 192.168.80.1
ip ssh version 2
!
!
ip access-list extended block_employees
 10 deny   ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 20 deny   ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 30 deny   ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
 40 deny   ip 192.168.30.0 0.0.0.255 192.168.50.0 0.0.0.255
 50 deny   tcp any any eq telnet
 60 deny   tcp any any eq 22
 70 permit ip 192.168.30.0 0.0.0.255 any
ip access-list extended block_wireless
 10 deny   ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
 20 deny   ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
 30 deny   ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
 40 deny   ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
 50 deny   tcp any any eq telnet
 60 deny   tcp any any eq 22
 70 permit ip 192.168.50.0 0.0.0.255 any
!
!
!
!
!
control-plane
!
!
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
!
!
!
line con 0
 exec-timeout 5 0
 password 7 06360E650859590B01
 logging synchronous
 login local
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 exec-timeout 5 0
 login local
 transport input ssh
!
!
!
!
!
!
end

Switch-1
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
ip arp inspection vlan 10,20,30,40,50,80,99-100
ip arp inspection validate src-mac
!
!
ip dhcp snooping vlan 10,20,30,40,50,80,99-100
ip dhcp snooping
ip domain-name nexus.local
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
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/4
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/5
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/6
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/7
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/8
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/9
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/10
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/11
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/12
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/13
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/14
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/15
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/16
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/17
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/18
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/19
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/20
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/21
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/22
 switchport access vlan 30
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/23
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 no cdp enable
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface FastEthernet0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 no cdp enable
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface GigabitEthernet0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface GigabitEthernet0/2
 switchport mode access
 switchport nonegotiate
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
 shutdown
 no cdp enable
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan100
 ip address 192.168.100.3 255.255.255.0
!
ip default-gateway 192.168.100.11
ip http server
ip http secure-server
ip ssh version 2
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

Switch-2
!
boot-start-marker
boot-end-marker
!
!
username admin privilege 15 secret 5 $1$nbO3$sSF9e0d1tVdgVxOgDXVoN1
no aaa new-model
system mtu routing 1500
ip arp inspection vlan 10,20,30,40,50,99-100
ip arp inspection validate src-mac
!
!
ip dhcp snooping vlan 10,20,30,40,50,99-100
ip dhcp snooping
ip domain-name nexus.local
login block-for 120 attempts 5 within 30
!
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
ip ssh version 2
!
!
!
!
!
interface Port-channel1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface FastEthernet0/1
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 74d0.2b26.fc93
 switchport port-security mac-address sticky bc24.11d8.8d70
 switchport port-security
 ip arp inspection trust
 no cdp enable
 ip dhcp snooping trust
!
interface FastEthernet0/2
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 74d0.2b28.b8bc
 switchport port-security mac-address sticky bc24.11cc.4ffc
 switchport port-security
 ip arp inspection trust
 no cdp enable
 ip dhcp snooping trust
!
interface FastEthernet0/3
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky a83c.a525.d879
 switchport port-security
 ip arp inspection trust
 no cdp enable
 ip dhcp snooping trust
!
interface FastEthernet0/4
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 3013.8b8f.05c6
 switchport port-security
 no cdp enable
!
interface FastEthernet0/5
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/6
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/7
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/8
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/9
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/10
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/11
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/12
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/13
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 3013.8b8f.05c6
 switchport port-security
 ip arp inspection trust
 no cdp enable
 ip dhcp snooping trust
!
interface FastEthernet0/14
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/15
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/16
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/17
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/18
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/19
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/20
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/21
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security
 no cdp enable
!
interface FastEthernet0/22
 switchport access vlan 50
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 40ed.009f.a768
 switchport port-security
 ip arp inspection trust
 no cdp enable
 ip dhcp snooping trust
!
interface FastEthernet0/23
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 no cdp enable
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface FastEthernet0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 no cdp enable
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface GigabitEthernet0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99,100
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface GigabitEthernet0/2
 switchport access vlan 100
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan100
 ip address 192.168.100.4 255.255.255.0
!
ip default-gateway 192.168.100.11
ip http server
ip http secure-server
!
!
!
line con 0
 exec-timeout 5 0
 password 7 073F20080A1E491713
 logging synchronous
 login local
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

!
end

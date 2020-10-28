# Introduction

This Tutorial show how to configure 3 Cisco Access Switches using Python 3 and Netmiko. 

## Requirements

1. Have Python3 and Netmiko install. 
2. We well need to configure manually the following parameters: SSH connection, username, password, mangement IP address. 

### Configuring the Cisco Devices

#### For Access Switch 1

```
enable
configure terminal
hostname ASw1
ip domain-name practice.com
enable secret cisco
username david privilege 15 secret cisco
interface vlan 1
description MGMT_VLAN
ip address 192.168.1.11 255.255.255.0
no shutdown
ip default-gateway 192.168.1.1
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
transport input ssh
login local
exit
write memory
```

#### For Access Switch 2

```
enable
configure terminal
hostname ASw2
ip domain-name practice.com
enable secret cisco
username david privilege 15 secret cisco
interface vlan 1
description MGMT_VLAN
ip address 192.168.1.12 255.255.255.0
no shutdown
ip default-gateway 192.168.1.1
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
transport input ssh
login local
exit
write memory
```

#### For Access Switch 3

```
enable
configure terminal
hostname ASw3
ip domain-name practice.com
enable secret cisco
username david privilege 15 secret cisco
interface vlan 1
description MGMT_VLAN
ip address 192.168.1.13 255.255.255.0
no shutdown
ip default-gateway 192.168.1.1
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
transport input ssh
login local
exit
write memory
```

### File with the Cisco Commands 

The name of the file is "iosv_l2_access_sw.txt" and it has the following commands:

```
!commands for Access switches
vlan 10
name Trunk_VLAN
!
Vlan 11
name DATA_VLAN
!
vlan 12
name PHONE_VLAN
!
vlan 19
name MGMT VLAN
!
! Configuring the Data ports
!
interface range g0/1-3, g1/0-3
switchport host
switchport access vlan 11
no shutdown
!
! Configuring the phone ports
!
interface range g2/0-3
switchport host
switchport access vlan 12
no shutdown
!
! Configuring trunking ports
!
interface range g3/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,11,12,19
no shutdown
```



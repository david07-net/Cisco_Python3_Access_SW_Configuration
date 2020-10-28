#Introduction

This Tutorial show how to configure 3 Cisco Access Switches using Python 3 and Netmiko. 

##Requirements

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


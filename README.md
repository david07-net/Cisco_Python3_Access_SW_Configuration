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

I used this commands because I think are the basic ones that you configure in any access switch. But We can always add as many commands as we need assuming that are applicable to all access switches. I saved the file in the same directory as the code so it the path to the file is not long you need to be careful with that. 

### Python Script

```Python
from netmiko import ConnectHandler

# Create a dictionary representing the device

iosv_l2_SW1 = {
    'device_type': 'cisco_ios',
    'ip': '192.168.1.11',
    'username': 'david',
    'password': 'cisco'
}

iosv_l2_SW2 = {
    'device_type': 'cisco_ios',
    'ip': '192.168.1.12',
    'username': 'david',
    'password': 'cisco'
}

iosv_l2_SW3 = {
    'device_type': 'cisco_ios',
    'ip': '192.168.1.13',
    'username': 'david',
    'password': 'cisco'
}

# Opent the file  with read permissions where the cisco commands are
# this will close the file when the program  stops running

with open('iosv_l2_access_sw') as f:
    lines = f.read().splitlines()      # read the file commands and separate into lines.
print (lines)                          # Print all the lines as a list


# Create a list with the device's dictionaries

access_switches_devices = [iosv_l2_SW1, iosv_l2_SW2, iosv_l2_SW3]


#Establish an SSH connection to the device by passing in the device dictionary.
#Since we have 3 Acess SW we will use a for loop command

for devices in access_switches_devices:
    net_connect = ConnectHandler(**devices)     # Connects to the device
    output = net_connect.send_config_set(lines) #Sent the commands
    print (output)                              #Print the commands sent to the Access switch
    
```



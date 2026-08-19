# P-2: Telnet and SSH Configuration

## Overview
This practical demonstrates the configuration and testing of remote access to a Cisco router using Telnet and SSH in Cisco Packet Tracer.

## Objective
To configure Telnet and SSH remote access on a Cisco router and verify connectivity from a PC.

## Network Topology
The topology consists of:

- Cisco 2911 Router
- Cisco Switch
- PC

## IP Addressing
| Device | Interface | IP Address | Subnet Mask |

| Router | GigabitEthernet 0/0 | 40.0.0.1 | 255.255.255.0 |
| PC | FastEthernet | 40.0.0.10 | 255.255.255.0 |

Default Gateway for PC:
40.0.0.1

1: Router Interface Configuration
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 40.0.0.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

2: PC IP Configuration

The PC was configured with:
IP Address: 40.0.0.10
Subnet Mask: 255.255.255.0
Default Gateway: 40.0.0.1

3: Telnet Configuration

VTY lines were configured for Telnet access.

Router# configure terminal
Router(config)# line vty 0 4
Router(config-line)# password telnet12
Router(config-line)# login
Router(config-line)# enable password 123456
Router(config)# transport input telnet
Router(config)# exit

Telnet Verification
Telnet connectivity was tested from the PC using:
PC> telnet 40.0.0.1


4: SSH Configuration
The router hostname and domain name were configured before generating RSA keys.

Router# enable
Router# configure terminal
Router(config)# hostname yess
yess(config)# ip domain-name jatinn
yess(config)# crypto key generate rsa

RSA key size: 1024 bits

A local user was configured:
yess(config)# username jatinn secret jatinn

VTY lines were configured for SSH:

yess(config)# line vty 0 4
yess(config-line)# login local
yess(config-line)# transport input ssh
yess(config-line)# exit

SSH Verification
SSH connectivity was tested from the PC using:

PC> ssh -l jatinn 40.0.0.1

Result
Telnet and SSH remote-access configurations were successfully practiced and tested using Cisco Packet Tracer.

Commit changes
⚠️ Ek import

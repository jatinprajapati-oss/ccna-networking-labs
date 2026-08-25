# NAT and PAT Configuration and Theory

## Introduction

NAT stands for **Network Address Translation**.

PAT stands for **Port Address Translation**.

NAT and PAT are important networking technologies used on routers to translate private IP addresses into public IP addresses.

They are mainly used when devices inside a private network need to communicate with devices on another network or the Internet.

Private IP addresses normally cannot be routed directly on the public Internet.

For example:

- Inside PC IP Address: `192.168.1.1`
- Router Inside IP Address: `192.168.1.10`
- Router Outside/Public IP Address: `89.203.12.46`
- Outside Server IP Address: `89.203.12.10`

When the inside PC sends traffic to the outside network, NAT can translate the private address into a public address.

NAT allows communication between private networks and public networks.

---

# 1. Why NAT Is Required

The main reason for NAT is that private IP addresses cannot normally be used directly on the public Internet.

Private IPv4 address ranges are:

```text
10.0.0.0 - 10.255.255.255
172.16.0.0 - 172.31.255.255
192.168.0.0 - 192.168.255.255
```
These addresses are used inside local networks.

For example:
PC IP Address: 192.168.1.1 ---- This is a private IP address.

If this PC wants to communicate with a public server, the router can translate the private IP address into a public IP address.

Example:

Before NAT:
```text
Source IP: 192.168.1.1
Destination IP: 89.203.12.10
```
After NAT:
```text
Source IP: 89.203.12.47
Destination IP: 89.203.12.10
```
The outside network sees the translated public address instead of the original private address.

---

# Main Benefits of NAT

NAT provides several benefits.

### 1. Saves Public IPv4 Addresses

- There are a limited number of public IPv4 addresses.
- PAT allows many private devices to share one public IP address.

Example:
```text
PC1 -> 192.168.1.1
PC2 -> 192.168.1.2
PC3 -> 192.168.1.3
```
All devices can access the Internet using one public IP address.

Example:

Public IP: 89.203.12.46

### 2. Hides Internal IP Addresses

Devices on the outside network do not directly see the private IP addresses.

For example:
```text
Internal Address: 192.168.1.1
Public Address:   89.203.12.47
```

- The outside network communicates with the public address.
- This provides basic address hiding, but NAT should not be considered a complete security solution.
- A firewall and proper security policies are still required.


### 3. Allows Private Networks to Access Public Networks

- NAT allows private devices to communicate with public servers and Internet services.

Example:
```text
Private Network
      |
192.168.1.0/24
      |
    Router
      |
Public Network
      |
Internet Server
```
---

# NAT Types

There are three main types of NAT:
```text
Static NAT
Dynamic NAT
PAT
```

## 1. Static NAT

Static NAT creates a permanent one-to-one mapping between a private IP address and a public IP address.

Example:
```text
Inside Local Address:  192.168.1.1
Inside Global Address: 89.203.12.47
```
The mapping is always:

192.168.1.1 <-> 89.203.12.47

This mapping does not change unless the configuration is removed or changed.

### Where Static NAT Is Used

Static NAT is commonly used for devices that must always be reachable from outside.

Examples include:
```text
Web servers
Mail servers
DNS servers
Application servers
```

### Static NAT Example

Network:

Inside PC:        192.168.1.1
Router Inside IP: 192.168.1.2
Router Outside IP: 89.203.12.47
Outside Server:   89.203.12.10

Router configuration: practical avaible on .pkt file above mention


## 2. Dynamic NAT

Dynamic NAT automatically translates private IP addresses using a pool of available public IP addresses.

Unlike Static NAT, the mapping is not permanently assigned to one device.

A device receives a public IP address from the NAT pool when it needs to communicate with the outside network.

### Dynamic NAT Working

Example private devices:
```text
PC1: 192.168.1.1
PC2: 192.168.1.2
PC3: 192.168.1.3
PC4: 192.168.1.4
```
Public NAT pool:
```text
89.203.12.50
89.203.12.51
89.203.12.52
89.203.12.53
89.203.12.54
```

When an inside device sends traffic:

192.168.1.1 -> NAT Pool -> 89.203.12.50

Another device may receive:

192.168.1.2 -> NAT Pool -> 89.203.12.51

The public address is selected dynamically from the available pool.

### Dynamic NAT Limit

Dynamic NAT requires enough public IP addresses for simultaneous users.

For example:

6 Public IP Addresses

Only six devices can receive a translation at the same time if each device requires a separate public address.

This is why PAT is more commonly used.

### Dynamic NAT Example

practical avaible on .pkt file



## 3. PAT

PAT stands for Port Address Translation.

PAT is also called:
```text
NAT Overload
NAT with Overload
Many-to-One NAT
```
PAT allows multiple inside devices to share one public IP address.

It identifies each connection by using different port numbers.

### Why PAT Is Important

PAT is very important because many users can access the Internet using only one public IP address.

Example:
```text
PC1: 192.168.1.1
PC2: 192.168.1.2
PC3: 192.168.1.3
PC4: 192.168.1.4
```
All devices can use:

Public IP: 89.203.12.46

The router uses different port numbers to identify different connections.

Example:
```text
192.168.1.1:3001 -> 89.203.12.46:10001
192.168.1.2:3002 -> 89.203.12.46:10002
192.168.1.3:3003 -> 89.203.12.46:10003
192.168.1.4:3004 -> 89.203.12.46:10004
```
The public IP address is the same.

The port number is different.

This allows the router to know which response belongs to which internal device.

### PAT Example

Network:
```text
PC1: 192.168.1.1
PC2: 192.168.1.2
PC3: 192.168.1.3
PC4: 192.168.1.4
```
```text
Router Inside:
192.168.1.10

Router Outside:
89.203.12.46

Outside Server:
89.203.12.10
```

Configure the Inside Interface
```text
enable
configure terminal

interface g0/0
ip address 192.168.1.10 255.255.255.0
no shutdown
ip nat inside
9.2 Configure the Outside Interface
interface g0/1
ip address 89.203.12.46 255.255.255.0
no shutdown
ip nat outside
```

Configure PAT Using the Router Outside Interface
```text
ip nat inside source list 1 interface g0/1 overload
```
The keyword: overload

enables PAT.

Multiple internal devices can now share the IP address of interface g0/1.

---


# Difference Between NAT and PAT

NAT can translate IP addresses.

PAT translates both:

IP Address + Port Number

Example NAT:

192.168.1.1 -> 89.203.12.47

Example PAT:

192.168.1.1:3001 -> 89.203.12.46:10001 , 192.168.1.2:3002 -> 89.203.12.46:10002

PAT is more efficient for conserving public IPv4 addresses.

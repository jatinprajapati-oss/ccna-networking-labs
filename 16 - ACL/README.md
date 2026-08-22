# Access Control List (ACL) Configuration

## Overview

Access Control List, also called ACL, is a set of rules used on routers and Layer 3 devices to control network traffic.

ACL checks packets based on rules and decides whether the packet should be:

- Permitted
- Denied

ACL is mainly used for:

- Traffic filtering
- Network security
- Access control
- Restricting communication between devices
- Allowing only required traffic
- Blocking unwanted traffic

In this practical, different types of ACLs are configured and tested:

1. Standard ACL
2. Extended ACL
3. Named ACL
4. ACL for SSH access

---

# What is an ACL?

ACL stands for:

```text
Access Control List
```

An ACL is a list of rules that a router checks when a packet enters or leaves an interface.

The router compares the packet with the ACL rules.

If the packet matches a rule:

```text
permit
```

the traffic is allowed.

If the packet matches a rule:

```text
deny
```

the traffic is blocked.

ACL rules are checked from top to bottom.

The first matching rule is applied.

After a packet matches one rule, the router stops checking the remaining rules.

---

# Basic ACL Working

The basic working process is:

```text
Packet arrives
       |
       v
Router checks ACL rule 1
       |
       v
Does it match?
   /         \
 Yes          No
  |            |
  v            v
Apply rule   Check next rule
Permit/Deny
```

ACL rules are processed in sequential order.

For example:

```text
Rule 1
Rule 2
Rule 3
Rule 4
```

The router checks Rule 1 first.

If the packet matches Rule 1, the router applies that rule.

The remaining rules are not checked.

---

# Important ACL Rule

Every ACL has an implicit deny at the end.

This means:

```text
deny any
```

is automatically present at the end of an ACL.

For example:

```text
access-list 1 permit host 40.0.0.1
```

The actual behavior is:

```text
permit host 40.0.0.1
deny any
```

Therefore, if you want to allow other traffic, you must explicitly configure:

```text
access-list 1 permit any
```

---

# Types of ACL

There are mainly two important types of numbered ACL:

1. Standard ACL
2. Extended ACL

ACL can also be configured using names.

This is called:

```text
Named ACL
```

---

# 1. Standard ACL

A Standard ACL filters traffic mainly based on the:

```text
Source IP Address
```

It does not check:

- Destination IP address
- Protocol
- TCP port number
- UDP port number

A Standard ACL is simple and is normally used when filtering traffic based only on the source.

---

# Standard ACL Number Range

Standard IPv4 ACLs generally use:

```text
1 - 99
```

and:

```text
1300 - 1999
```

Example:

```text
access-list 1 permit host 40.0.0.1
```

This allows traffic coming from:

```text
40.0.0.1
```

---

# Standard ACL Example

Suppose there are two PCs:

```text
PC0 = 40.0.0.1
PC1 = 40.0.0.2
```

We want to block PC1:

```text
40.0.0.2
```

but allow all other devices.

Configuration:

```text
access-list 1 deny host 40.0.0.2
access-list 1 permit any
```

The first rule blocks:

```text
40.0.0.2
```

The second rule allows:

```text
all other traffic
```

---

# Applying Standard ACL

Creating an ACL is not enough.

The ACL must be applied to an interface.

Example:

```text
interface g0/1
ip access-group 1 out
```

This applies ACL number 1 to the outgoing traffic on interface:

```text
GigabitEthernet 0/1
```

---

# ACL Direction

ACL can be applied in two directions:

```text
in
```

and:

```text
out
```

---

# Inbound ACL

Inbound means:

The packet is checked when it enters the router interface.

Example:

```text
ip access-group 1 in
```

Traffic flow:

```text
Device
   |
   v
Router Interface
   |
ACL Check
   |
   v
Router Processing
```

---

# Outbound ACL

Outbound means:

The packet is checked when it leaves the router interface.

Example:

```text
ip access-group 1 out
```

Traffic flow:

```text
Router
   |
   v
ACL Check
   |
   v
Outgoing Interface
   |
   v
Destination
```

---

# Standard ACL Practical

In this practical, the following Standard ACL configuration is used:

```text
Router(config)#access-list 1 deny host 40.0.0.2
Router(config)#access-list 1 permit any
Router(config)#int g0/1
Router(config-if)#ip access-group 1 out
```

Explanation:

```text
access-list 1
```

Creates or configures ACL number 1.

```text
deny host 40.0.0.2
```

Blocks traffic from the source IP address:

```text
40.0.0.2
```

```text
permit any
```

Allows traffic from all other source addresses.

```text
int g0/1
```

Enters the interface configuration.

```text
ip access-group 1 out
```

Applies ACL 1 to outgoing traffic.

---

# 2. Extended ACL

An Extended ACL provides more detailed traffic filtering than a Standard ACL.

An Extended ACL can filter traffic based on:

- Source IP address
- Destination IP address
- Protocol
- TCP
- UDP
- ICMP
- Port number

Because of this, Extended ACL provides more control over network traffic.

---

# Extended ACL Number Range

Extended IPv4 ACLs generally use:

```text
100 - 199
```

and:

```text
2000 - 2699
```

---

# Extended ACL Example

Suppose:

```text
PC0 = 40.0.0.1
PC1 = 40.0.0.2

Server0 = 50.0.0.1
Server1 = 50.0.0.2
```

An Extended ACL can control exactly which device can access which server.

For example:

```text
permit icmp host 40.0.0.1 host 50.0.0.1 echo
```

This allows ICMP echo packets from:

```text
40.0.0.1
```

to:

```text
50.0.0.1
```

---

# ICMP in Extended ACL

ICMP is commonly used by the:

```text
ping
```

command.

Important ICMP types include:

```text
echo
```

and:

```text
echo-reply
```

Example:

```text
permit icmp host 40.0.0.1 host 50.0.0.1 echo
```

Allows the ping request.

Example:

```text
permit icmp host 50.0.0.1 host 40.0.0.1 echo-reply
```

Allows the ping reply.

Both directions may need to be considered depending on where and how the ACL is applied.

---

# TCP Port Filtering

Extended ACL can filter traffic using TCP port numbers.

For example:

```text
permit tcp host 40.0.0.1 host 50.0.0.1 eq 80
```

This allows TCP traffic from:

```text
40.0.0.1
```

to:

```text
50.0.0.1
```

using port:

```text
80
```

Port 80 is commonly used for:

```text
HTTP
```

---

# Common Port Numbers

| Protocol | Port |
|---|---|
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |
| Telnet | 23 |
| FTP | 21 |
| DNS | 53 |

These ports can be controlled using an Extended ACL.

---

# Extended ACL Practical

In this practical, a named Extended ACL is configured.

Configuration:

```text
Router#conf t
Router(config)#ip access-list extended jatin
```

This creates an Extended ACL with the name:

```text
jatin
```

---

# Allowing ICMP Traffic

The following command allows ICMP echo packets:

```text
permit icmp host 40.0.0.1 host 50.0.0.1 echo
```

This means:

```text
Source: 40.0.0.1
Destination: 50.0.0.1
Protocol: ICMP
Type: Echo
```

---

# Allowing HTTP Traffic

The following command allows HTTP traffic:

```text
permit tcp host 40.0.0.1 host 50.0.0.1 eq 80
```

Explanation:

```text
permit tcp
```

Allows TCP traffic.

```text
host 40.0.0.1
```

Specifies the source device.

```text
host 50.0.0.1
```

Specifies the destination device.

```text
eq 80
```

Allows traffic using port 80.

---

# Full Extended ACL Configuration

The practical configuration includes rules similar to:

```text
Router(config)#ip access-list extended jatin

Router(config-ext-nacl)#permit icmp host 40.0.0.1 host 50.0.0.1 echo
Router(config-ext-nacl)#permit tcp host 40.0.0.1 host 50.0.0.1 eq 80

Router(config-ext-nacl)#permit tcp host 40.0.0.1 host 50.0.0.2 eq 80
Router(config-ext-nacl)#permit tcp host 40.0.0.1 host 50.0.0.2 eq 21

Router(config-ext-nacl)#permit icmp host 40.0.0.1 host 50.0.0.1 echo-reply
Router(config-ext-nacl)#permit icmp host 40.0.0.2 host 50.0.0.1 echo
Router(config-ext-nacl)#permit icmp host 40.0.0.2 host 50.0.0.2 echo-reply

Router(config-ext-nacl)#permit tcp host 40.0.0.2 host 50.0.0.2 eq 80
Router(config-ext-nacl)#permit tcp host 40.0.0.2 host 50.0.0.2 eq 21

Router(config-ext-nacl)#deny ip any any
```

---

# Understanding deny ip any any

The command:

```text
deny ip any any
```

blocks all IP traffic that does not match a previous permit rule.

Therefore, ACL rules above it define the only traffic that is allowed.

Example:

```text
permit tcp host 40.0.0.1 host 50.0.0.1 eq 80
deny ip any any
```

Result:

```text
Only HTTP traffic from 40.0.0.1 to 50.0.0.1 is allowed.
All other IP traffic is denied.
```

---

# Applying the Extended ACL

After creating the ACL, it must be applied to an interface.

Example:

```text
Router(config)#interface g0/0
Router(config-if)#ip access-group jatin in
```

Explanation:

```text
ip access-group jatin
```

Applies the named ACL:

```text
jatin
```

```text
in
```

Checks packets when they enter the interface.

---

# 3. Named ACL

A Named ACL uses a name instead of an ACL number.

Example:

```text
ip access-list standard jatin
```

or:

```text
ip access-list extended jatin
```

The name makes ACL configuration easier to understand.

Instead of remembering:

```text
ACL 1
ACL 101
```

we can use a meaningful name such as:

```text
jatin
```

---

# Named Standard ACL

Example:

```text
Router(config)#ip access-list standard jatin
Router(config-std-nacl)#permit host 40.0.0.2
Router(config-std-nacl)#exit
```

This creates a Standard ACL named:

```text
jatin
```

and permits:

```text
40.0.0.2
```

---

# Applying Named ACL

Example:

```text
interface g0/0
ip access-group jatin in
```

This applies the named ACL to incoming traffic.

---

# 4. ACL for SSH Access

ACL can also be used to control who is allowed to remotely access a router using SSH.

In this practical, only a specific host is permitted to establish an SSH connection.

First, a Standard Named ACL is created:

```text
ip access-list standard jatin
permit host 40.0.0.2
exit
```

This allows only:

```text
40.0.0.2
```

---

# SSH Configuration

The router requires a local username.

Example:

```text
username jatin secret 12345
```

A domain name is required before generating RSA keys.

Example:

```text
ip domain-name yess
```

RSA keys are generated using:

```text
crypto key generate rsa
```

Example key size:

```text
1024
```

---

# Configuring SSH on VTY Lines

The VTY lines are configured using:

```text
line vty 0 4
```

Local authentication is enabled:

```text
login local
```

Only SSH is allowed:

```text
transport input ssh
```

The ACL is applied using:

```text
access-class jatin in
```

---

# Full SSH ACL Configuration

The complete configuration is:

```text
enable
configure terminal

ip access-list standard jatin
permit host 40.0.0.2
exit

username jatin secret 12345

ip domain-name yess

crypto key generate rsa
1024

ip ssh version 2

line vty 0 4
login local
transport input ssh
access-class jatin in

end
```

---

# SSH ACL Working

The ACL controls which IP address is allowed to access the router through SSH.

For example:

```text
permit host 40.0.0.2
```

Result:

```text
40.0.0.2  -> SSH Access Allowed
```

Other hosts are blocked by the ACL.

Traffic flow:

```text
PC
 |
 | SSH Request
 v
Router
 |
 | ACL Check
 v
Is Source IP Allowed?
 |
 +---- Yes ----> SSH Login
 |
 +---- No -----> Access Denied
```

---

# access-group vs access-class

These two commands are used for different purposes.

## ip access-group

Example:

```text
ip access-group 1 in
```

This is used to apply an ACL to a router interface for packet filtering.

---

## access-class

Example:

```text
access-class jatin in
```

This is commonly used on VTY lines to control remote management access such as SSH or Telnet.

---

# Standard ACL vs Extended ACL

| Feature | Standard ACL | Extended ACL |
|---|---|---|
| Source IP | Yes | Yes |
| Destination IP | No | Yes |
| Protocol | No | Yes |
| Port Number | No | Yes |
| Filtering Control | Basic | Detailed |
| Complexity | Simple | More Complex |

---

# Standard ACL Example Summary

Configuration:

```text
access-list 1 deny host 40.0.0.2
access-list 1 permit any

interface g0/1
ip access-group 1 out
```

Result:

```text
40.0.0.2 -> Denied
Other Hosts -> Permitted
```

---

# Extended ACL Example Summary

Configuration example:

```text
ip access-list extended jatin
permit icmp host 40.0.0.1 host 50.0.0.1 echo
permit tcp host 40.0.0.1 host 50.0.0.1 eq 80
deny ip any any
```

Result:

```text
40.0.0.1 -> 50.0.0.1

ICMP Echo -> Allowed
HTTP Port 80 -> Allowed
Other IP Traffic -> Denied
```

---

# Important ACL Commands

## View IP Access Lists

```text
show access-lists
```

This displays the configured ACLs.

---

## View Running Configuration

```text
show running-config
```

This displays the current router configuration.

---

## View Interface Configuration

```text
show ip interface
```

This can help check whether an ACL is applied to an interface.

---

## View SSH Status

```text
show ip ssh
```

This displays SSH information.

---

# Important ACL Points

1. ACL rules are checked from top to bottom.

2. The first matching rule is applied.

3. After a match, the router stops checking further rules.

4. ACL contains an implicit:

```text
deny any
```

at the end.

5. Standard ACL mainly checks the source IP address.

6. Extended ACL can check source, destination, protocol, and port number.

7. ACL must be applied to an interface or VTY line to take effect.

8. ACL can be applied in:

```text
in
```

or:

```text
out
```

direction.

9. `ip access-group` is used for interface traffic filtering.

10. `access-class` is used to restrict management access on VTY lines.

11. Named ACL uses a readable name instead of a number.

12. Extended ACL provides more detailed security control than Standard ACL.

13. Rule order is very important.

14. A broad permit rule placed before a deny rule can cause the deny rule to never be reached.

---

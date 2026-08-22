# P-11: VTP (VLAN Trunking Protocol)

## Overview

This practical demonstrates the implementation of **VTP (VLAN Trunking Protocol)** using multiple Cisco switches connected through trunk links.

The network contains multiple switches and multiple VLANs. VTP is used to manage and distribute VLAN information between switches within the same VTP domain.

In this practical, one switch is configured as a **VTP Server** and the remaining switches are configured as **VTP Clients**.

The VLANs created on the VTP Server are automatically advertised to the VTP Client switches through trunk links.

---

# What is VTP?

VTP stands for **VLAN Trunking Protocol**.

It is a Cisco proprietary Layer 2 protocol used to manage VLAN configurations across multiple switches.

Normally, when multiple switches are present in a network, VLANs must be created manually on every switch.

For example, if a network has 10 switches and VLAN 100 needs to be created, the administrator would normally have to create VLAN 100 manually on all 10 switches.

VTP simplifies this process.

With VTP, VLAN information can be distributed automatically from one switch to other switches in the same VTP domain.

---

# Why is VTP Used?

VTP is used to simplify VLAN management in a network containing multiple switches.

Without VTP:

- VLANs need to be created manually on every switch.
- VLAN configuration can become time-consuming.
- Configuration mistakes may occur.
- Managing a large network becomes difficult.

With VTP:

- VLAN information can be distributed automatically.
- VLAN management becomes easier.
- VLAN configuration is centralized.
- Administrative work is reduced.

---

# Main Purpose of VTP

The main purpose of VTP is to maintain consistent VLAN information across multiple Cisco switches.

When VLAN information is created or modified on the VTP Server, the information can be advertised to VTP Client switches through trunk links.

This helps maintain the same VLAN database across the network.

---

# VTP Domain

A VTP Domain is a group of switches that share VLAN information with each other.

All switches participating in the same VTP environment must use the same VTP domain name.

Example:

```text
Switch(config)#vtp domain jatin
```

In this practical, the switches are configured in the same VTP domain.

Only switches that belong to the same VTP domain can properly exchange VTP advertisements.

---

# VTP Modes

VTP supports different operating modes.

## 1. VTP Server

The VTP Server is responsible for managing VLAN information.

A VTP Server can:

- Create VLANs.
- Modify VLANs.
- Delete VLANs.
- Advertise VLAN information to other switches.
- Synchronize VLAN information with VTP Clients.

Example configuration:

```text
Switch(config)#vtp mode server
```

In this practical, Switch 1 is configured as the VTP Server.

---

## 2. VTP Client

A VTP Client receives VLAN information from the VTP Server.

A VTP Client receives VLAN updates through VTP advertisements.

Example configuration:

```text
Switch(config)#vtp mode client
```

In this practical, the remaining switches are configured as VTP Clients.

---

## 3. VTP Transparent Mode

A switch in VTP Transparent mode maintains its own VLAN configuration.

The switch does not automatically synchronize its VLAN database with the VTP Server.

Example:

```text
Switch(config)#vtp mode transparent
```

---

# VTP Version

VTP supports different versions.

In this practical, **VTP Version 2** is used.

Example:

```text
Switch(config)#vtp version 2
```

---

# VTP Server Configuration

The following commands are used to configure the VTP Server:

```text
Switch(config)#vtp domain jatin
Switch(config)#vtp password 123
Switch(config)#vtp mode server
Switch(config)#vtp version 2
```

## Explanation

### VTP Domain

```text
vtp domain jatin
```

This command configures the VTP domain name.

All switches participating in the VTP environment should use the same domain.

### VTP Password

```text
vtp password 123
```

This command configures a VTP password.

The switches should use the same password for proper VTP communication.

### VTP Server Mode

```text
vtp mode server
```

This command configures the switch as a VTP Server.

The server is responsible for managing VLAN information.

### VTP Version

```text
vtp version 2
```

This command configures VTP Version 2.

---

# VLAN Configuration

After configuring the VTP Server, VLANs are created on the server.

In this practical, the following VLANs are created:

- VLAN 100
- VLAN 200
- VLAN 300
- VLAN 400

Configuration:

```text
Switch(config)#vlan 100
Switch(config-vlan)#name blue

Switch(config)#vlan 200
Switch(config-vlan)#name green

Switch(config)#vlan 300
Switch(config-vlan)#name yellow

Switch(config)#vlan 400
Switch(config-vlan)#name red
```

The VLAN information created on the VTP Server is advertised to the VTP Clients.

---

# VLAN Details

| VLAN ID | VLAN Name |
|---|---|
| 100 | Blue |
| 200 | Green |
| 300 | Yellow |
| 400 | Red |

These VLANs are used to logically separate devices into different broadcast domains.

---

# VTP Client Configuration

The remaining switches are configured as VTP Clients.

Example configuration:

```text
Switch(config)#vtp domain jatin
Switch(config)#vtp password 123
Switch(config)#vtp mode client
Switch(config)#vtp version 2
```

The following configuration should match the VTP Server:

- VTP Domain
- VTP Password
- VTP Version

Once the switches are connected through trunk links, VLAN information from the VTP Server is synchronized with the clients.

---

# Trunk Configuration

VTP advertisements are exchanged between switches through trunk links.

Therefore, the switch-to-switch connections must be configured as trunk ports.

Example:

```text
Switch(config)#interface range g0/1 - 2
Switch(config-if-range)#switchport mode trunk
Switch(config-if-range)#switchport trunk allowed vlan all
```

---

# What is a Trunk Port?

A trunk port is a switch port that can carry traffic from multiple VLANs.

An access port normally carries traffic for only one VLAN.

Trunk ports are commonly used for:

- Switch to Switch connections.
- Switch to Router connections.
- Switch to Layer 3 Switch connections.

In this practical, trunk ports are used between switches so that VLAN information and traffic can travel across the network.

---

# Access Port Configuration

Ports connected to PCs are configured as access ports.

Each access port belongs to a specific VLAN.

Example:

```text
Switch(config)#interface g0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 100
```

This configuration assigns the connected device to VLAN 100.

---

# How VTP Works

The VTP operation can be understood in the following steps:

1. A VTP domain is configured.
2. Switches are configured with the same VTP domain.
3. One switch is configured as a VTP Server.
4. Other switches are configured as VTP Clients.
5. Trunk links are configured between the switches.
6. VLANs are created on the VTP Server.
7. The VTP Server generates VLAN advertisements.
8. VTP advertisements travel through trunk links.
9. VTP Clients receive the VLAN information.
10. The VLAN database is synchronized across the switches.

---

# VTP Advertisement

VTP uses advertisements to distribute VLAN information between switches.

When a VLAN is:

- Created
- Modified
- Deleted

the VLAN database changes.

The VTP Server advertises the updated VLAN information to other switches in the same VTP domain.

The VTP Clients receive the updated information and synchronize their VLAN database.

---

# VTP Configuration Revision Number

VTP uses a configuration revision number to identify changes in the VLAN database.

When the VLAN database is modified, the configuration revision number can change.

Switches compare VTP information and synchronize their VLAN databases according to the received VTP advertisements.

The configuration revision number is an important concept when working with VTP.

Incorrect or unexpected VTP information with a higher revision number can affect VLAN synchronization. Therefore, VTP should be configured carefully.

---

# VTP Requirements

For proper VTP operation, the following requirements should be met:

- Switches should be connected through trunk links.
- The VTP domain should match.
- The VTP password should match if configured.
- Compatible VTP versions should be used.
- Switches should use the correct VTP mode.
- VTP advertisements must be able to travel between the switches.

---

# Advantages of VTP

VTP provides several advantages:

- Simplifies VLAN management.
- Reduces manual configuration.
- Saves administrative time.
- Helps maintain consistent VLAN information.
- Reduces the possibility of VLAN configuration errors.
- Useful in networks containing multiple switches.
- VLANs can be centrally managed.

---

# Limitations of VTP

VTP should be configured carefully.

Some limitations include:

- Incorrect configuration can affect multiple switches.
- Proper VTP domain configuration is required.
- Trunk connectivity is required between switches.
- Incorrect revision information can affect VLAN synchronization.
- VTP should be carefully planned before implementation in large networks.

---

# Network Topology

The practical contains multiple switches connected together through trunk links.

The general topology is:

```text
              Router
                |
             Switch 1
                |
             Switch 2
                |
             Switch 3
                |
             Switch 4
```

Multiple PCs are connected to different switches.

The PCs are divided into different VLANs.

The VLANs used in the network are:

```text
VLAN 100
VLAN 200
VLAN 300
VLAN 400
```

---

# VLAN Distribution

The VTP Server distributes VLAN information to the VTP Clients.

The process is:

```text
        VTP Server
            |
            | VLAN Advertisements
            v
      Trunk Connections
            |
            v
       VTP Clients
            |
            v
VLAN Database Synchronization
```

---

# Configuration Summary

## VTP Server

```text
Switch(config)#vtp domain jatin
Switch(config)#vtp password 123
Switch(config)#vtp mode server
Switch(config)#vtp version 2
```

## VLAN Configuration

```text
Switch(config)#vlan 100
Switch(config-vlan)#name blue

Switch(config)#vlan 200
Switch(config-vlan)#name green

Switch(config)#vlan 300
Switch(config-vlan)#name yellow

Switch(config)#vlan 400
Switch(config-vlan)#name red
```

## VTP Client

```text
Switch(config)#vtp domain jatin
Switch(config)#vtp password 123
Switch(config)#vtp mode client
Switch(config)#vtp version 2
```

## Trunk Configuration

```text
Switch(config)#interface range g0/1 - 2
Switch(config-if-range)#switchport mode trunk
Switch(config-if-range)#switchport trunk allowed vlan all
```

---

# Verification Commands

The following commands can be used to verify the VTP and VLAN configuration.

## Check VTP Status

```text
Switch#show vtp status
```

This command displays:

- VTP Version
- VTP Domain Name
- VTP Operating Mode
- Configuration Revision Number
- Number of Existing VLANs

---

## Check VLAN Information

```text
Switch#show vlan brief
```

This command displays:

- VLAN IDs
- VLAN Names
- VLAN Status
- Ports assigned to VLANs

---

## Check Trunk Information

```text
Switch#show interfaces trunk
```

This command displays information about configured trunk interfaces.

---

# Technologies Used

- Cisco Packet Tracer
- Cisco Switches
- VLAN
- VTP
- Trunking
- Access Ports
- IEEE 802.1Q

---

# Key Learning Outcomes

After completing this practical, the following concepts were understood:

- What is VTP.
- Why VTP is used.
- VTP Domain.
- VTP Server.
- VTP Client.
- VTP Transparent Mode.
- VTP Version 2.
- VLAN creation.
- VLAN synchronization.
- Trunk ports.
- Access ports.
- VTP advertisements.
- VTP revision number.
- VTP verification commands.

---

# Practical Result

The VTP configuration was successfully implemented using Cisco Packet Tracer.

One switch was configured as a VTP Server, while the remaining switches were configured as VTP Clients.

The following VLANs were created on the VTP Server:

```text
VLAN 100 - Blue
VLAN 200 - Green
VLAN 300 - Yellow
VLAN 400 - Red
```

The VLAN information was distributed from the VTP Server to the VTP Client switches through trunk connections.

The VLAN configuration was successfully synchronized across the network.

---

# Conclusion

VTP is used to simplify VLAN management across multiple Cisco switches.

Instead of manually creating the same VLAN on every switch, VLAN information can be managed centrally using a VTP Server and distributed to VTP Client switches.

In this practical, a VTP domain was configured, multiple VLANs were created, trunk links were configured between switches, and VLAN information was distributed across the VTP network.

# P-? EtherChannel Configuration Using PAgP

## Overview

EtherChannel is a networking technology that allows multiple physical Ethernet links to be combined into a single logical link called a **Port-Channel**.

Instead of using only one physical connection between two switches, multiple links can work together as one logical connection. This increases available bandwidth and also provides redundancy.

In this practical, four FastEthernet interfaces are bundled together using **PAgP (Port Aggregation Protocol)** to create an EtherChannel between two Cisco switches.

---

# What is EtherChannel?

EtherChannel is a Layer 2 or Layer 3 technology used to combine multiple physical network interfaces into one logical interface.

For example, if four Ethernet links connect two switches, EtherChannel can combine those four links into one logical connection.

The logical interface created is called:

```text
Port-Channel
```

The switches treat multiple physical interfaces as one logical link.

---

# Why Do We Use EtherChannel?

EtherChannel provides several important advantages.

## 1. Increased Bandwidth

Multiple physical links are combined together.

For example:

```text
4 × 100 Mbps links
```

can provide higher total available bandwidth compared to using only one 100 Mbps link.

---

## 2. Redundancy

If one physical link fails, the remaining links in the EtherChannel can continue carrying traffic.

This improves network availability.

---

## 3. Load Balancing

Traffic can be distributed across multiple physical links in the EtherChannel.

This helps utilize the available links efficiently.

---

## 4. Reduced STP Blocking

Normally, when multiple redundant links exist between switches, Spanning Tree Protocol may block some links to prevent loops.

With EtherChannel, the group of physical links is treated as one logical link by STP.

Therefore, the member interfaces can be used together instead of being individually blocked.

---

# EtherChannel Protocols

There are mainly two negotiation protocols used for EtherChannel.

## 1. PAgP

PAgP stands for:

```text
Port Aggregation Protocol
```

PAgP is a Cisco proprietary protocol.

It is used to automatically negotiate and create an EtherChannel between Cisco switches.

PAgP modes include:

- Desirable
- Auto

### Desirable Mode

The switch actively tries to negotiate and form an EtherChannel.

Example:

```text
channel-group 1 mode desirable
```

### Auto Mode

The switch waits for the other device to initiate PAgP negotiation.

Example:

```text
channel-group 1 mode auto
```

### Valid PAgP Combination

```text
Desirable + Auto
```

or

```text
Desirable + Desirable
```

### Invalid Combination

```text
Auto + Auto
```

Two switches configured with Auto mode will not actively negotiate an EtherChannel.

---

## 2. LACP

LACP stands for:

```text
Link Aggregation Control Protocol
```

LACP is an IEEE standard protocol defined by IEEE 802.3ad / 802.1AX.

LACP modes include:

- Active
- Passive

Valid combinations include:

```text
Active + Passive
```

and

```text
Active + Active
```

---

# EtherChannel Modes

## PAgP Modes

| Mode | Description |
|---|---|
| Desirable | Actively negotiates EtherChannel |
| Auto | Waits for the other switch to initiate negotiation |

## LACP Modes

| Mode | Description |
|---|---|
| Active | Actively initiates LACP negotiation |
| Passive | Waits for LACP negotiation |

## Static EtherChannel

A static EtherChannel can also be configured without a negotiation protocol.

Example:

```text
channel-group 1 mode on
```

In this mode, both sides must be configured correctly because there is no protocol negotiation.

---

# Topology

The topology contains:

```text
PC0
 |
Switch0
 ||||
 ||||  Four FastEthernet Links
 ||||
Switch1
 |
PC1
```

The four physical links between Switch0 and Switch1 are combined into:

```text
Port-Channel 1
```

---

# Devices Used

- 2 Cisco 2960 Switches
- 2 PCs
- 4 FastEthernet links between the switches
- Cisco Packet Tracer

---

# EtherChannel Configuration

The EtherChannel is configured using the following interfaces:

```text
FastEthernet 0/1
FastEthernet 0/2
FastEthernet 0/3
FastEthernet 0/4
```

These four interfaces are grouped into:

```text
Channel Group 1
```

which creates:

```text
Port-Channel 1
```

---

# Switch0 Configuration

Switch0 is configured in PAgP Desirable mode.

```text
enable
configure terminal

interface range fastethernet 0/1-4
channel-protocol pagp
channel-group 1 mode desirable

end
```

Explanation:

```text
interface range fastethernet 0/1-4
```

Selects all four FastEthernet interfaces.

```text
channel-protocol pagp
```

Specifies PAgP as the EtherChannel negotiation protocol.

```text
channel-group 1 mode desirable
```

Places the interfaces into Channel Group 1 and actively negotiates the EtherChannel.

---

# Switch1 Configuration

Switch1 is configured in PAgP Auto mode.

```text
enable
configure terminal

interface range fastethernet 0/1-4
channel-protocol pagp
channel-group 1 mode auto

end
```

Explanation:

```text
mode auto
```

Switch1 waits for PAgP negotiation.

Since Switch0 is configured as:

```text
mode desirable
```

the EtherChannel negotiation can successfully occur.

The combination is:

```text
Switch0: Desirable
Switch1: Auto
```

Result:

```text
EtherChannel Formed Successfully
```

---

# EtherChannel Verification

After configuration, the EtherChannel can be verified using:

```text
show etherchannel summary
```

This command displays information about:

- Channel Groups
- Port-Channel interfaces
- EtherChannel protocol
- Member interfaces
- Interface status

Example command:

```text
Switch# show etherchannel summary
```

The EtherChannel should show the physical interfaces as members of the Port-Channel.

---

# Important EtherChannel Requirements

The interfaces participating in an EtherChannel should have compatible configurations.

Important settings include:

- Same interface type
- Same speed
- Same duplex configuration
- Same switchport mode
- Compatible VLAN configuration
- Same trunk configuration when trunking is used
- Same EtherChannel protocol on both sides

If the configurations do not match, the EtherChannel may fail to form correctly.

---

# EtherChannel and Spanning Tree Protocol

Spanning Tree Protocol sees the EtherChannel as one logical link.

For example, even if four physical interfaces are used:

```text
Fa0/1
Fa0/2
Fa0/3
Fa0/4
```

STP treats the EtherChannel as a single logical interface:

```text
Port-Channel 1
```

This allows multiple physical links to work together while helping avoid Layer 2 switching loops.

---

# Advantages of EtherChannel

EtherChannel provides the following benefits:

- Increased bandwidth
- Link redundancy
- Better availability
- Traffic load balancing
- Reduced STP blocking of redundant links
- Logical management of multiple interfaces
- Improved network performance
- Fault tolerance

---

# PAgP Mode Combination Used in This Practical

This practical uses:

| Switch | Protocol | Mode |
|---|---|---|
| Switch0 | PAgP | Desirable |
| Switch1 | PAgP | Auto |

The negotiation is:

```text
Desirable  <------>  Auto
```

This successfully forms an EtherChannel.

---

# Port-Channel

After EtherChannel configuration, the group of physical interfaces is represented by a logical interface.

In this practical:

```text
Channel Group: 1
```

creates:

```text
Port-Channel 1
```

The physical interfaces work together as members of the same logical EtherChannel.

---

# Commands Used

## Switch0

```text
enable
configure terminal
interface range fastethernet 0/1-4
channel-protocol pagp
channel-group 1 mode desirable
end
```

## Switch1

```text
enable
configure terminal
interface range fastethernet 0/1-4
channel-protocol pagp
channel-group 1 mode auto
end
```

## Verification

```text
show etherchannel summary
```

---

# Conclusion

In this practical, an EtherChannel was successfully configured between two Cisco switches using the PAgP protocol.

Four FastEthernet interfaces were combined into a single logical Port-Channel.

Switch0 was configured in:

```text
PAgP Desirable Mode
```

and Switch1 was configured in:

```text
PAgP Auto Mode
```

The EtherChannel provides increased bandwidth, redundancy, load balancing, and improved network reliability.

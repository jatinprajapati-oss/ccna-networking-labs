# HSRP (Hot Standby Router Protocol)

## Introduction

HSRP stands for **Hot Standby Router Protocol**. It is a Cisco protocol used to provide **default gateway redundancy** in a network.

In a normal network, a PC uses one router as its default gateway. If that router fails, the PC may lose connectivity to other networks.

HSRP solves this problem by using multiple routers. These routers work together and create one **Virtual Gateway**. End devices such as PCs use the **Virtual IP Address** as their default gateway instead of using the physical IP address of only one router.

If the current router fails, another router automatically takes over. This provides high availability and reduces network downtime.

---

# Why HSRP Is Used

HSRP is mainly used to provide a backup for the default gateway.

Without HSRP:

PC → Router → Other Network

If the router fails:

PC → No Default Gateway → Communication with remote networks may stop

With HSRP:

PC → Virtual IP → Active Router → Other Network

                     ↓

                Standby Router

If the Active Router fails, the Standby Router automatically becomes Active.

HSRP provides:

- Default gateway redundancy
- High availability
- Automatic failover
- Reduced downtime
- Network reliability
- Backup gateway support

---

# Main Components of HSRP

## Active Router

The Active Router is the router that currently forwards traffic for the HSRP group.

Only one router is normally Active in one HSRP group.

Example:

Router0 = Active Router

Router1 = Standby Router

Traffic flow:

PC → Virtual IP → Router0 → Destination Network

The Active Router is responsible for forwarding traffic sent to the Virtual Gateway.

---

## Standby Router

The Standby Router is the backup router.

It continuously monitors the Active Router.

If the Active Router fails, the Standby Router automatically becomes the new Active Router.

Example:

Before failure:

Router0 = Active

Router1 = Standby

If Router0 fails:

Router0 = Down

Router1 = Active

The network can continue working through Router1.

---

## Virtual IP Address

The Virtual IP Address is one of the most important parts of HSRP.

The Virtual IP Address is configured as the **Default Gateway** on PCs and other end devices.

Example:

Router0 Physical IP = 40.0.0.1

Router1 Physical IP = 40.0.0.2

HSRP Virtual IP = 40.0.0.3

PC configuration:

IP Address: 40.0.0.10

Subnet Mask: 255.255.255.0

Default Gateway: 40.0.0.3

The PC uses 40.0.0.3 as its gateway.

It does not need to know whether Router0 or Router1 is currently Active.

---

## HSRP Group

Routers participating in HSRP are placed in an HSRP group.

Each HSRP group has a group number.

Example:

standby 1 ip 40.0.0.3

Here:

1 = HSRP Group Number

40.0.0.3 = Virtual IP Address

Routers participating in the same HSRP group use the same group number and Virtual IP configuration.

---

# How HSRP Works

HSRP works in the following steps:

Step 1: Multiple routers are connected to the same network.

Step 2: HSRP is configured on the routers.

Step 3: A Virtual IP Address is configured.

Step 4: HSRP selects one router as the Active Router.

Step 5: Another router becomes the Standby Router.

Step 6: PCs use the Virtual IP Address as their default gateway.

Step 7: The Active Router forwards traffic.

Step 8: The Standby Router monitors the Active Router.

Step 9: If the Active Router fails, the Standby Router becomes Active.

Step 10: Traffic continues through the new Active Router.

The PC does not need to change its default gateway.

---

# HSRP Priority

HSRP uses priority to decide which router should become the Active Router.

The router with the higher priority normally becomes Active.

The default HSRP priority is:

100

Example:

Router0 Priority = 150

Router1 Priority = 100

Result:

Router0 = Active

Router1 = Standby

Because Router0 has the higher priority.

Configuration:

standby 1 priority 150

The priority value can be used to select the preferred Active Router.

---

# HSRP Tie Breaker

What happens if both routers have the same priority?

Example:

Router0 Priority = 100

Router1 Priority = 100

In this situation, HSRP uses the router IP address as a tie-breaker.

The router with the higher IP address can become the Active Router.

Example:

Router0 = 40.0.0.1

Router1 = 40.0.0.2

Both routers have the same priority.

Router1 has the higher IP address.

Therefore, Router1 can become Active.

---

# HSRP Preempt

Preempt allows a higher-priority router to take back the Active role after it comes back online.

Example:

Router0 Priority = 150

Router1 Priority = 100

Initially:

Router0 = Active

Router1 = Standby

Now Router0 fails.

Router1 becomes Active.

Later, Router0 comes back online.

Without Preempt:

Router1 may remain Active.

Router0 may remain Standby.

With Preempt:

Router0 can become Active again because it has the higher priority.

Configuration:

standby 1 preempt

Preempt is useful when a specific router is preferred as the Active Router.

---

# Example of Preempt

Initial situation:

Router0

Priority = 150

Role = Active

Router1

Priority = 100

Role = Standby

Router0 fails:

Router1 changes from Standby to Active.

Router0 comes back online.

If Preempt is enabled:

Router0 = Active

Router1 = Standby

This happens because Router0 has a higher priority.

---

# HSRP States

HSRP routers move through different states.

The main HSRP states are:

## Initial State

The router is starting the HSRP process.

It is not yet fully participating in HSRP.

---

## Learn State

The router learns information about the HSRP Virtual IP Address.

---

## Listen State

The router listens for HSRP messages from other routers.

It knows about the Active and Standby routers but is not currently Active or Standby.

---

## Speak State

The router sends and receives HSRP Hello messages.

It participates in the election process.

---

## Standby State

The router is selected as the backup router.

It monitors the Active Router.

If the Active Router fails, the Standby Router can become Active.

---

## Active State

The router is currently forwarding traffic for the HSRP Virtual IP Address.

Only one router is normally Active in an HSRP group.

---

# HSRP Hello Messages

HSRP routers communicate with each other using Hello messages.

Hello messages help routers determine:

- Which router is Active
- Which router is Standby
- Whether the Active Router is still available
- Whether a router failure has occurred

The Standby Router continuously receives information about the Active Router.

If it stops receiving Hello messages for the required time, it can assume that the Active Router has failed.

The Standby Router can then become Active.

---

# HSRP Timers

HSRP uses timers to control Hello messages and failure detection.

The main timers are:

## Hello Timer

The Hello Timer controls how often HSRP Hello messages are sent.

## Hold Timer

The Hold Timer controls how long a router waits before considering another router unavailable.

If the Hold Timer expires and no Hello messages are received from the Active Router, failover can occur.

Example configuration:

standby 1 timers 1 3

In this example:

Hello Timer = 1 second

Hold Timer = 3 seconds

Lower timer values can provide faster failover but can increase control traffic.

---

# HSRP Virtual MAC Address

HSRP also uses a Virtual MAC Address.

The Virtual IP Address is associated with a virtual gateway.

The Active Router handles traffic for that virtual gateway.

When a failover occurs, the new Active Router takes over the forwarding role.

This allows end devices to continue using the same gateway information.

The PC does not need to manually change its gateway when the Active Router changes.

---

# Basic HSRP Topology

Example:

PC0 -------- Switch -------- Router0

                              |

                              |

                            Router1

Router0 and Router1 are configured with HSRP.

Example addressing:

Router0 = 40.0.0.1

Router1 = 40.0.0.2

Virtual IP = 40.0.0.3

PC0 = 40.0.0.10

PC1 = 40.0.0.20

Default Gateway for PCs = 40.0.0.3

The PCs use only the Virtual IP as their gateway.

---

# Basic HSRP Configuration

Example configuration for Router0:

enable

configure terminal

interface g0/0

ip address 40.0.0.1 255.255.255.0

no shutdown

standby 1 ip 40.0.0.3

standby 1 priority 150

standby 1 preempt

end

Router0 has a higher priority.

Therefore, it is preferred as the Active Router.

---

# Router1 Configuration

enable

configure terminal

interface g0/0

ip address 40.0.0.2 255.255.255.0

no shutdown

standby 1 ip 40.0.0.3

standby 1 priority 100

standby 1 preempt

end

Router1 has a lower priority.

Therefore, it normally becomes the Standby Router.

---

# PC Configuration

The PCs must use the HSRP Virtual IP as their default gateway.

Example:

PC0:

IP Address: 40.0.0.10

Subnet Mask: 255.255.255.0

Default Gateway: 40.0.0.3

PC1:

IP Address: 40.0.0.20

Subnet Mask: 255.255.255.0

Default Gateway: 40.0.0.3

Important:

Do not use Router0 physical IP as the main HSRP gateway.

Do not use Router1 physical IP as the main HSRP gateway.

Use the Virtual IP Address:

40.0.0.3

---

# HSRP Failover

Failover is the process where the Standby Router becomes Active when the Active Router fails.

Initial condition:

Router0 = Active

Router1 = Standby

Traffic flow:

PC → Virtual IP → Router0 → Destination

Now Router0 fails.

The HSRP Standby Router detects the failure.

Then:

Router1 = Active

New traffic flow:

PC → Same Virtual IP → Router1 → Destination

The PC still uses the same Virtual IP.

No manual gateway change is required.

---

# Testing HSRP Failover

After configuring HSRP, first verify the Active and Standby routers.

Use:

show standby

or:

show standby brief

Example:

Router0 should show:

Active

Router1 should show:

Standby

Then test connectivity using ping.

Example:

ping 40.0.0.20

Now simulate a failure on the Active Router.

Example:

configure terminal

interface g0/0

shutdown

After the Active Router interface is shut down, check the other router.

Use:

show standby brief

The Standby Router should become Active.

Then test connectivity again.

If the network routing is correctly configured, communication should continue.

---

# Interface Tracking

HSRP can also track an important interface.

This is useful when the router is still running but one of its important network connections fails.

Example:

Router0 is Active.

Router0 is connected to the LAN and an upstream network.

Suppose the upstream connection fails.

Router0 may still be running and may still remain Active.

This can cause traffic problems.

Interface Tracking helps solve this problem.

Example command:

standby 1 track g0/1

HSRP can monitor the interface.

If the tracked interface fails, the router's priority can be reduced.

This can allow another router to become Active.

---

# Priority Decrement with Tracking

Example:

standby 1 priority 150

standby 1 track g0/1 60

Initial priority:

150

If interface g0/1 fails:

150 - 60 = 90

Now suppose Router1 has priority:

100

Router1 now has a higher priority than Router0.

Router1 can become the Active Router.

This provides better redundancy because HSRP can respond to important interface failures.

---

# Multiple HSRP Groups

A network can use multiple HSRP groups.

Example:

Group 1:

Router0 = Active

Router1 = Standby

Group 2:

Router1 = Active

Router0 = Standby

Different groups can use different Virtual IP Addresses.

Example:

Group 1 Virtual IP = 40.0.0.3

Group 2 Virtual IP = 40.0.0.4

Multiple HSRP groups can be used for basic load sharing.

Different devices can use different Virtual IP Addresses.

This allows traffic to use different Active Routers.

---

# Load Sharing Using Multiple HSRP Groups

Example:

PC Group A:

Default Gateway = Virtual IP Group 1

Active Router = Router0

PC Group B:

Default Gateway = Virtual IP Group 2

Active Router = Router1

This means:

Router0 handles traffic for Group 1.

Router1 handles traffic for Group 2.

Both routers can still provide backup support.

---

# HSRP and Routing

HSRP provides default gateway redundancy.

However, HSRP does not automatically create routes to remote networks.

Routing is still required.

Routing can be configured using:

- Static Routing
- RIP
- OSPF
- EIGRP
- Other routing protocols

Example static route:

ip route 0.0.0.0 0.0.0.0 50.0.0.2

This creates a default route.

HSRP and routing protocols have different purposes.

HSRP:

Provides Default Gateway Redundancy.

Routing:

Provides paths between different networks.

---

# Important HSRP Commands

Configure Virtual IP:

standby 1 ip 40.0.0.3

Configure Priority:

standby 1 priority 150

Enable Preempt:

standby 1 preempt

Configure Interface Tracking:

standby 1 track g0/1

Configure Tracking with Priority Decrement:

standby 1 track g0/1 60

Configure Timers:

standby 1 timers 1 3

Check HSRP Status:

show standby

Check Brief HSRP Information:

show standby brief

Check Current Configuration:

show running-config

---

# Understanding show standby

The command:

show standby

displays detailed HSRP information.

It can show:

- HSRP Group Number
- Current HSRP State
- Active Router
- Standby Router
- Virtual IP Address
- Priority
- Hello Timer
- Hold Timer
- Preemption Status

This command is useful for troubleshooting and verification.

---

# Understanding show standby brief

The command:

show standby brief

provides a short summary of HSRP information.

It can quickly show:

- Interface
- Group Number
- Priority
- Current State
- Active Router
- Standby Router
- Virtual IP

Example concept:

Interface: GigabitEthernet0/0

Group: 1

Priority: 150

State: Active

Virtual IP: 40.0.0.3

This makes it easy to check which router is currently Active.

---

# Complete Working Example

Network:

PC0

|

Switch

/     \

Router0   Router1

\         /

Remote Network Router

Addressing:

Router0 LAN IP = 40.0.0.1

Router1 LAN IP = 40.0.0.2

Virtual IP = 40.0.0.3

PC0 = 40.0.0.10

PC1 = 40.0.0.20

PC Default Gateway = 40.0.0.3

HSRP Group:

1

Priority:

Router0 = 150

Router1 = 100

Normal operation:

Router0 = Active

Router1 = Standby

Traffic:

PC → 40.0.0.3 → Router0 → Remote Network

If Router0 fails:

Router1 changes:

Standby → Active

New traffic:

PC → 40.0.0.3 → Router1 → Remote Network

The PC continues using the same Virtual IP Address.

---

# Advantages of HSRP

HSRP provides many advantages:

- Provides default gateway redundancy
- Provides automatic failover
- Improves network availability
- Reduces downtime
- Provides a backup router
- PCs use one common Virtual IP
- No manual gateway change is required during failover
- Supports priority configuration
- Supports Preempt
- Supports interface tracking
- Supports multiple HSRP groups
- Can provide basic load sharing using multiple groups
- Improves network reliability

---

# Limitations of HSRP

HSRP also has some limitations:

- It is primarily associated with Cisco networking environments
- Only one router actively forwards traffic for one HSRP group
- The Standby Router is mainly used as a backup for that group
- HSRP does not replace routing protocols
- Proper routing is still required
- Incorrect configuration can cause failover problems
- Priority and Preempt should be configured carefully
- Interface tracking may be required for better redundancy

---

# Best Practices for HSRP

1. Use a Virtual IP Address as the default gateway for end devices.

2. Configure a higher priority on the preferred Active Router.

3. Configure a lower priority on the backup router.

4. Use Preempt if the preferred router should regain the Active role after recovery.

5. Use Interface Tracking for important uplink connections.

6. Verify the configuration using:

show standby

and:

show standby brief

7. Test failover before using the configuration in an important network.

8. Make sure proper routing is configured between all required networks.

9. Document the following information:

- HSRP Group Number
- Virtual IP Address
- Active Router
- Standby Router
- Priority Values
- Tracked Interfaces

---

# Physical IP Address vs Virtual IP Address

Physical IP Address:

Belongs to a specific router.

Example:

Router0 = 40.0.0.1

Router1 = 40.0.0.2

Virtual IP Address:

Belongs to the HSRP group.

Example:

40.0.0.3

The PCs use:

40.0.0.3

as their default gateway.

This is the main concept of HSRP.

The physical router can change, but the Virtual IP used by the PC remains the same.

---

# Active Router vs Standby Router

Active Router:

- Currently forwards traffic
- Handles the Virtual Gateway
- Sends traffic toward other networks

Standby Router:

- Acts as a backup
- Monitors the Active Router
- Becomes Active when the Active Router fails

Example:

Router0:

Priority = 150

Role = Active

Router1:

Priority = 100

Role = Standby

If Router0 fails:

Router1:

Old Role = Standby

New Role = Active

---

---

# Final HSRP Working Process

Multiple Routers

↓

HSRP Configuration

↓

HSRP Group Created

↓

Virtual IP Address Configured

↓

Router Election

↓

One Router Becomes Active

↓

Another Router Becomes Standby

↓

PC Uses Virtual IP as Default Gateway

↓

Active Router Forwards Traffic

↓

Standby Router Monitors Active Router

↓

Active Router Fails

↓

Standby Router Detects Failure

↓

Standby Router Becomes Active

↓

Traffic Continues Through the New Active Router

↓

The PC Still Uses the Same Virtual IP Address

---

# Conclusion

HSRP is a First Hop Redundancy Protocol used to provide **default gateway redundancy**.

It allows multiple routers to work together and provide one Virtual IP Address to end devices.

One router works as the **Active Router** and forwards traffic.

Another router works as the **Standby Router** and waits to take over if the Active Router fails.

End devices use the Virtual IP Address as their default gateway.

If the Active Router fails, the Standby Router automatically becomes Active.


HSRP improves network reliability and availability by ensuring that a single router failure does not completely remove the default gateway from the network.

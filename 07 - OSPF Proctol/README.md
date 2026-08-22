# OSPF (Open Shortest Path First)

## Table of Contents

# 1. Introduction

OSPF stands for **Open Shortest Path First**.
It is a dynamic routing protocol used by routers to exchange routing information and automatically determine the best path to destination networks.
OSPF is one of the most commonly used Interior Gateway Protocols in enterprise networks. It is designed to support medium and large networks where scalability, fast convergence, and efficient routing are important.
Unlike static routing, where routes must be configured manually, OSPF allows routers to automatically learn information about remote networks.

---

# 2. What is OSPF?

OSPF is a **link-state dynamic routing protocol**..

A router running OSPF collects information about the network topology and shares this information with other OSPF routers.
Each router creates a map of the network topology and uses this information to calculate the best path to every destination.

OSPF selects routes based on the **lowest cost**.

The basic OSPF process can be represented as:

```text
Discover Neighbors
        ↓
Exchange Routing Information
        ↓
Build Link-State Database
        ↓
Run SPF Algorithm
        ↓
Calculate Best Path
        ↓
Install Route in Routing Table
```

---

# 3. Why OSPF is Used

OSPF is used because it provides several advantages over simpler routing protocols.

It is suitable for networks that require:

- Dynamic routing
- Fast convergence
- Scalability
- Hierarchical network design
- Multiple routing paths
- VLSM support
- CIDR support
- Authentication
- Efficient routing updates

OSPF is commonly used in enterprise networks where the network contains multiple routers and multiple subnets.

---

# 4. OSPF as an Interior Gateway Protocol

OSPF is an **Interior Gateway Protocol (IGP)**.

An IGP is used to exchange routing information within a single autonomous system.

Examples of IGPs include:

- RIP
- OSPF
- EIGRP
- IS-IS

OSPF is generally used inside an organization's network rather than for routing between large independent organizations on the Internet.

---

# 5. OSPF Working

OSPF works by allowing routers to discover neighboring routers and exchange information about connected networks.

The general working process is:

1. OSPF is enabled on router interfaces.
2. Routers send Hello packets.
3. Neighboring routers discover each other.
4. Routers establish an OSPF neighbor relationship.
5. Routing information is exchanged.
6. Each router builds a Link-State Database.
7. The SPF algorithm is executed.
8. The best paths are calculated.
9. Best routes are installed in the routing table.

This process allows routers to dynamically learn about remote networks.

---

# 6. Link-State Routing Protocol

OSPF is called a **link-state routing protocol** because routers exchange information about the state of their network links.

A link can represent a router interface and its connection to another network or router.

Instead of simply telling neighbors about the best route, OSPF shares information that allows routers to build a complete view of the network topology.

This information is stored in the:

```text
Link-State Database (LSDB)
```

Routers within the same OSPF area should maintain a consistent view of the network topology.

---

# 7. Shortest Path First Algorithm

OSPF uses the **Shortest Path First (SPF)** algorithm.

The SPF algorithm is also known as the **Dijkstra algorithm**.

The router uses the topology information stored in the Link-State Database to calculate the best path to each destination.

The route with the lowest total OSPF cost is selected.

For example:

```text
Path A:
Router1 → Router2 → Router4
Total Cost = 20

Path B:
Router1 → Router3 → Router4
Total Cost = 50
```

OSPF selects:

```text
Path A
```

because it has the lower total cost.

---

# 8. OSPF Metric and Cost

OSPF uses **Cost** as its routing metric.

The cost of a route is based mainly on interface bandwidth.

The basic formula is:

```text
OSPF Cost = Reference Bandwidth / Interface Bandwidth
```

A route with a lower total cost is preferred.

For example:

```text
Path 1 = Cost 10
Path 2 = Cost 25
```

OSPF selects:

```text
Path 1
```

because the cost is lower.

The total route cost is calculated by adding the costs of the links along the path.

---

# 9. OSPF Process ID

When configuring OSPF on a Cisco router, a process ID is used.

Example:

```text
router ospf 1
```

Here:

```text
1
```

is the OSPF process ID.

The process ID is locally significant on the router.

This means different routers can use different OSPF process IDs and still become OSPF neighbors.

For example:

```text
Router1:
router ospf 1

Router2:
router ospf 10
```

The routers can still form an OSPF neighbor relationship if other required parameters are compatible.

---

# 10. Router ID

Every OSPF router requires a unique **Router ID**.

The Router ID is a 32-bit value represented in an IPv4 address format.

Example:

```text
1.1.1.1
```

or:

```text
192.168.1.1
```

The Router ID identifies the router within the OSPF domain.

A unique Router ID is important because OSPF uses it to identify routers and perform functions such as DR and BDR election.

A Router ID can be manually configured:

```text
router ospf 1
router-id 1.1.1.1
```

---

# 11. OSPF Areas

OSPF supports a hierarchical network design using **areas**.

An area is a logical grouping of routers and networks.

Instead of placing all routers into one large routing domain, a network can be divided into multiple OSPF areas.

Example:

```text
Area 1
   │
   │
Area 0
   │
   │
Area 2
```

Using areas helps improve scalability and reduces the amount of routing information that must be processed by every router.

Common area numbers include:

```text
Area 0
Area 1
Area 2
Area 3
```

---

# 12. Backbone Area

**Area 0** is called the OSPF Backbone Area.

It is the central area of an OSPF multi-area network.

Other areas normally connect through Area 0.

Example:

```text
Area 1
   │
   │
Area 0
   │
   │
Area 2
```

Area 0 is important because inter-area traffic normally passes through the backbone.

---

# 13. Types of OSPF Routers

OSPF routers can perform different roles.

## Internal Router

An Internal Router has all of its OSPF interfaces in the same area.

Example:

```text
All interfaces → Area 1
```

---

## Backbone Router

A Backbone Router has at least one interface connected to Area 0.

Example:

```text
Router Interface → Area 0
```

---

## Area Border Router

An Area Border Router, also called an **ABR**, connects multiple OSPF areas.

Example:

```text
Area 1
   │
  ABR
   │
Area 0
```

An ABR maintains information about the areas it connects.

---

## Autonomous System Boundary Router

An Autonomous System Boundary Router, also called an **ASBR**, connects the OSPF domain to another routing domain.

An ASBR can redistribute routes from another routing source into OSPF.

---

# 14. OSPF Neighbor Relationship

Before routers exchange complete routing information, they must establish an OSPF neighbor relationship.

Routers use **Hello packets** to discover neighboring OSPF routers.

For routers to successfully become neighbors, several important parameters must be compatible.

These include:

- Area ID
- Hello interval
- Dead interval
- Network type
- Authentication settings
- Subnet connectivity

If important parameters do not match, routers may fail to establish an OSPF adjacency.

---

# 15. OSPF Neighbor States

OSPF routers pass through several states while establishing an adjacency.

The common states are:

```text
Down
↓
Init
↓
2-Way
↓
ExStart
↓
Exchange
↓
Loading
↓
Full
```

## Down State

The router has not received Hello packets from the neighbor.

---

## Init State

The router receives a Hello packet from another router.

---

## 2-Way State

The routers recognize each other as neighbors.

On some network types, routers may remain in the 2-Way state depending on their relationship with the DR and BDR.

---

## ExStart State

The routers begin the process of exchanging database information.

They determine which router will begin the exchange.

---

## Exchange State

The routers exchange Database Description information.

---

## Loading State

The routers request and receive missing link-state information.

---

## Full State

The routers have successfully synchronized their Link-State Databases.

A Full state indicates a complete OSPF adjacency.

---

# 16. OSPF Packet Types

OSPF uses several packet types.

## 1. Hello Packet

Used to:

- Discover neighboring routers
- Maintain neighbor relationships
- Participate in DR and BDR election

---

## 2. Database Description Packet

Used to exchange summaries of the Link-State Database.

---

## 3. Link-State Request Packet

Used to request specific link-state information.

---

## 4. Link-State Update Packet

Used to send Link-State Advertisements.

---

## 5. Link-State Acknowledgment Packet

Used to acknowledge received Link-State Advertisements.

---

# 17. Link-State Advertisements

OSPF routers use **Link-State Advertisements**, commonly called **LSAs**, to share topology information.

LSAs contain information about routers, networks, and links.

Routers use the received LSA information to build the:

```text
Link-State Database
```

When the network topology changes, updated link-state information can be propagated through the OSPF area.

---

# 18. OSPF Database

OSPF maintains several important data structures.

## Neighbor Table

Contains information about directly connected OSPF neighbors.

It can be viewed using:

```text
show ip ospf neighbor
```

---

## Link-State Database

Contains the topology information learned through OSPF.

It can be viewed using:

```text
show ip ospf database
```

---

## Routing Table

Contains the best routes selected by the SPF algorithm.

It can be viewed using:

```text
show ip route
```

---

# 19. Designated Router and Backup Designated Router

On multi-access networks, OSPF can elect a:

- Designated Router (DR)
- Backup Designated Router (BDR)

The DR helps reduce the number of OSPF adjacencies required between routers on the same network.

Example:

```text
        Router2
           │
Router1 ── DR ── Router3
           │
          BDR
```

The BDR acts as a backup to the DR.

If the DR fails, the BDR can take over the designated router role.

---

# 20. OSPF Network Types

OSPF can operate over different network types.

Common network types include:

- Broadcast
- Non-Broadcast
- Point-to-Point
- Point-to-Multipoint

Different network types can affect how neighbors are discovered and whether DR and BDR elections occur.

---

# 21. OSPF Authentication

OSPF supports authentication between neighboring routers.

Authentication helps prevent unauthorized routers from participating in OSPF routing.

Common authentication methods include:

## Simple Password Authentication

A password is used to authenticate OSPF communication.

---

## Message Digest Authentication

Message Digest authentication provides stronger protection by using a hash-based authentication method.

For authentication to work correctly, compatible authentication settings must be configured on both sides of the OSPF connection.

If authentication settings do not match, OSPF neighbors may fail to establish an adjacency.

---

# 22. Wildcard Mask

OSPF uses a **wildcard mask** when specifying networks.

A wildcard mask is the inverse of a subnet mask.

Example:

```text
Subnet Mask:    255.255.255.0
Wildcard Mask:  0.0.0.255
```

Another example:

```text
Subnet Mask:    255.255.0.0
Wildcard Mask:  0.0.255.255
```

A network statement may look like:

```text
network 192.168.1.0 0.0.0.255 area 0
```

This enables OSPF on matching interfaces and places them into the specified area.

---

# 23. OSPF Route Types

Routes learned through OSPF can be identified by different route codes.

## O

An intra-area route.

The destination network is within the same OSPF area.

Example:

```text
O
```

---

## O IA

An inter-area route.

The destination network was learned from another OSPF area.

Example:

```text
O IA
```

---

## O E1

An external OSPF route where the internal OSPF cost is included in the route calculation.

---

## O E2

An external OSPF route where the external metric is the primary metric used.

---

# 24. OSPF Administrative Distance

The default administrative distance of OSPF is:

```text
110
```

Administrative distance is used by a router to determine the trustworthiness of routing information when the same destination is learned from different routing sources.

A lower administrative distance is generally preferred.

---

# 25. Basic OSPF Configuration

OSPF can be configured by creating an OSPF routing process.

Example:

```text
Router> enable
Router# configure terminal
Router(config)# router ospf 1
```

Advertise a network:

```text
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
```

Another network:

```text
Router(config-router)# network 10.0.0.0 0.0.0.255 area 0
```

The router will enable OSPF on interfaces that match the configured network statements.

---

# 26. OSPF Verification Commands

The following commands are useful for verifying OSPF.

## Display OSPF Process Information

```text
show ip ospf
```

---

## Display OSPF Neighbors

```text
show ip ospf neighbor
```

This command helps verify whether neighboring routers have formed an OSPF relationship.

---

## Display OSPF Interface Information

```text
show ip ospf interface brief
```

---

## Display Detailed OSPF Interface Information

```text
show ip ospf interface
```

---

## Display OSPF Database

```text
show ip ospf database
```

---

## Display OSPF Routes

```text
show ip route ospf
```

---

## Display Complete Routing Table

```text
show ip route
```

---

## Display Running Configuration

```text
show running-config
```

---

# 27. OSPF Route Identification

Routes learned through OSPF are displayed in the Cisco routing table with OSPF route codes.

Examples:

```text
O
O IA
O E1
O E2
```

Example routing table entry:

```text
O    192.168.2.0/24 [110/20] via 10.0.0.2
```

Where:

```text
O       = OSPF route
110     = Administrative Distance
20      = OSPF Metric
10.0.0.2 = Next-hop router
```

---

# 28. Advantages of OSPF

OSPF provides several advantages:

- Supports large networks
- Provides fast convergence
- Uses cost as a metric
- Supports VLSM
- Supports CIDR
- Supports hierarchical routing using areas
- Supports authentication
- Sends routing information efficiently
- Automatically adapts to topology changes

---

# 29. Limitations of OSPF

Some limitations include:

- More complex than RIP
- Requires more planning
- Requires more CPU resources
- Requires more memory
- Multi-area configuration can be complex
- Troubleshooting can be more difficult for beginners

---

# 30. RIP vs OSPF

| Feature | RIP | OSPF |
|---|---|---|
| Routing Type | Distance Vector | Link-State |
| Metric | Hop Count | Cost |
| Maximum Network Size | Small Networks | Medium and Large Networks |
| Convergence | Slower | Faster |
| Maximum Hop Count | 15 | No 15-hop limitation |
| Areas | Not Supported | Supported |
| Algorithm | Distance Vector Method | SPF Algorithm |
| Scalability | Low | High |
| Authentication | Supported in RIPv2 | Supported |
| VLSM | RIPv2 Supports | Supported |

---

# RIP (Routing Information Protocol)

## What is RIP?
- RIP (Routing Information Protocol) is a dynamic routing protocol used by routers to exchange routing information and determine the best path to destination networks.

- RIP automatically updates the routing table when network information is exchanged between neighboring routers.

## How RIP Works
- RIP uses **hop count** as its routing metric.

- A hop represents one router that a packet passes through to reach its destination.

For example:
Router A → Router B → Router C

The path from Router A to Router C has 2 hops.

- RIP selects the route with the lowest hop count as the preferred route.
- RIP Metric
- RIP uses hop count to select routes.

0 hops – Directly connected network
1 hop – One router away
2 hops – Two routers away
3 hops – Three routers away
Maximum – 15 hops

A hop count of 16 is considered unreachable.

Versions of RIP
RIP has different versions:

RIPv1
- Classful routing protocol
- Does not support VLSM
- Does not support CIDR
- Uses broadcast for routing updates

RIPv2
- Classless routing protocol
- Supports VLSM
- Supports CIDR
- Supports subnet information
- Uses multicast address 224.0.0.9
- Supports authentication

RIP Timers
RIP uses timers to maintain and update routing information.

Common RIP timers include:
- Update Timer – Sends routing updates periodically.
- Invalid Timer – Determines when a route becomes invalid.
- Hold-down Timer – Prevents unstable route information from being accepted temporarily.
- Flush Timer – Determines when an invalid route is removed from the routing table.

Advantages of RIP
- Easy to configure
- Simple to understand
- Suitable for small networks
- Automatically exchanges routing information
- Requires less configuration compared to some advanced routing protocols

Limitations of RIP
- Maximum hop count is 15
- Not suitable for large networks
- Convergence is slower than many modern routing protocols
- Routing decisions are based only on hop count
- Frequent routing updates can create unnecessary network traffic

RIPv2
RIPv2 is commonly preferred over RIPv1 because it supports classless routing and provides additional routing features.
RIPv2 allows routers to exchange subnet information and supports modern IP addressing requirements such as VLSM.

Common RIP Commands

router rip
Enables RIP configuration mode.

version 2
Enables RIPv2.

network <network-address>
Advertises a network through RIP.

no auto summary
Disables automatic route summarization.

show ip route
Displays the router's routing table.

show ip protocols
Displays information about the configured routing protocols.

RIP Route Identification
In the Cisco routing table, routes learned through RIP are identified by:

R
For example:
R    192.168.2.0/24
This indicates that the route was learned through RIP.

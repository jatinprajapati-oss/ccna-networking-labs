# P-3: Static Routing (2 Routers)

## Overview
This practical demonstrates the configuration of static routing between two Cisco routers using Cisco Packet Tracer.

## Objective
To configure static routes between two different networks using two Cisco routers and verify connectivity between the end devices.

## Network Topology
The topology consists of:

- 2 Cisco 2911 Routers
- 2 PCs
- Router-to-router connection

Topology:
PC0 ── Router0 ── Router1 ── PC1

IP Addressing

PC0	    40.0.0.10
Router0	40.0.0.1
Router0	50.0.0.1
Router1	50.0.0.2
Router1	60.0.0.1

PC1	60.0.0.10	60.0.0.0/24
Static Route Configuration

Router0
Router0 was configured with a static route to the 60.0.0.0/24 network through Router1.

Router(config)# ip route 60.0.0.0 255.255.255.0 50.0.0.2

Router1
Router1 was configured with a static route to the 40.0.0.0/24 network through Router0.

Router(config)# ip route 40.0.0.0 255.255.255.0 50.0.0.1
Static Routing Table
Router	Destination Network	Next Hop
Router0	60.0.0.0/24	50.0.0.2
Router1	40.0.0.0/24	50.0.0.1

Verification
Static routes can be verified using:
Router# show ip route

Connectivity can be tested using:
PC0> ping 60.0.0.10
and:
PC1> ping 40.0.0.10

Result
Static routing was successfully configured between Router0 and Router1, allowing communication between the 40.0.0.0/24 and 60.0.0.0/24 networks.

Then:

Commit c

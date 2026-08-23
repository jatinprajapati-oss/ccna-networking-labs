# DHCP (Dynamic Host Configuration Protocol)

## 1. Introduction

DHCP stands for **Dynamic Host Configuration Protocol**.

DHCP is a network protocol that automatically provides IP configuration to devices on a network.

Normally, every device needs some network settings to communicate with other devices. These settings include:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

Without DHCP, the network administrator must configure these settings manually on every device.

For example:

```text
PC1
IP Address: 40.0.0.1
Subnet Mask: 255.255.255.0
Default Gateway: 40.0.0.10
DNS Server: 8.8.8.8
```

If there are many computers, manually configuring every device can take a lot of time and may create configuration errors.

DHCP solves this problem by automatically assigning network configuration to devices.

---

# 2. Why DHCP is Used

DHCP is used to automatically configure devices when they connect to a network.

Instead of manually assigning:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

the DHCP server automatically provides these settings.

Example:

```text
DHCP Server
      |
      |
    Switch
   /  |  \
 PC1 PC2 PC3

PC1 → Automatically receives IP address
PC2 → Automatically receives IP address
PC3 → Automatically receives IP address
```

This makes network management easier.

---

# 3. Advantages of DHCP

DHCP provides many advantages.

- Automatically assigns IP addresses.
- Reduces manual configuration.
- Saves network administrator time.
- Reduces human configuration errors.
- Helps prevent duplicate IP addresses.
- Provides centralized IP address management.
- Automatically provides DNS server information.
- Makes large networks easier to manage.
- Supports temporary devices joining and leaving the network.

---

# 4. Main Components of DHCP

The main components of DHCP are:

## DHCP Server

A DHCP Server is responsible for assigning IP addresses and other network configuration to clients.

The DHCP server maintains information such as:

- Available IP addresses.
- DHCP pools.
- Excluded addresses.
- Default gateway.
- DNS server.
- Lease information.

Example:

```text
DHCP Server

Network: 40.0.0.0/24

Available IP Pool:
40.0.0.11 - 40.0.0.254

Default Gateway:
40.0.0.10

DNS Server:
8.8.8.8
```

---

## DHCP Client

A DHCP Client is a device that requests network configuration from a DHCP server.

Examples of DHCP clients include:

- PCs
- Laptops
- Mobile devices
- Printers
- IP Phones
- Other network devices

The client is configured to obtain an IP address automatically.

Example:

```text
PC
↓
IP Configuration
↓
DHCP
```

After selecting DHCP, the device requests network configuration from the DHCP server.

---

## DHCP Pool

A DHCP Pool is a collection or range of IP addresses that the DHCP server can assign to clients.

Example:

```text
Network:
40.0.0.0/24

Available IP Addresses:
40.0.0.1 - 40.0.0.254
```

The DHCP server assigns addresses from the pool.

Example:

```text
PC1 → 40.0.0.11
PC2 → 40.0.0.12
PC3 → 40.0.0.13
```

---

## DHCP Excluded Address

An excluded address is an IP address that the DHCP server must not assign to DHCP clients.

Excluded addresses are commonly used for devices that need static IP addresses.

Examples include:

- Router
- Server
- Switch
- Printer
- Firewall

Example:

```text
Router IP Address:
40.0.0.10

Server IP Address:
40.0.0.20
```

These addresses should not be assigned to normal DHCP clients.

Cisco command:

```cisco
Router(config)#ip dhcp excluded-address 40.0.0.1 40.0.0.10
```

This means DHCP will not assign addresses from:

```text
40.0.0.1
to
40.0.0.10
```

---

# 5. DHCP Working Process

DHCP uses a process called **DORA**.

DORA stands for:

```text
D → Discover
O → Offer
R → Request
A → Acknowledgment
```

The complete DHCP process is:

```text
DHCP Client
     |
     | DHCP Discover
     ↓
DHCP Server
     |
     | DHCP Offer
     ↓
DHCP Client
     |
     | DHCP Request
     ↓
DHCP Server
     |
     | DHCP Acknowledgment
     ↓
DHCP Client receives IP configuration
```

---

# 6. Step 1: DHCP Discover

When a client starts and does not have an IP address, it sends a DHCP Discover message.

The client is asking:

```text
Is there any DHCP server available?
```

Example:

```text
PC
 |
 | DHCP Discover
 |
 ↓
Network
```

Because the client does not yet know the DHCP server address, the Discover message is normally sent as a broadcast.

---

# 7. Step 2: DHCP Offer

The DHCP server receives the Discover message.

The DHCP server checks the available IP addresses in its DHCP pool.

Then the server offers an available IP address to the client.

Example:

```text
DHCP Server

Available IP:
40.0.0.11

Subnet Mask:
255.255.255.0

Default Gateway:
40.0.0.10

DNS Server:
8.8.8.8
```

The server sends:

```text
DHCP Offer
```

---

# 8. Step 3: DHCP Request

The client receives the DHCP Offer.

The client then sends a DHCP Request message.

The client is basically saying:

```text
I want to use this offered IP address.
```

Example:

```text
PC
 |
 | DHCP Request
 |
 ↓
DHCP Server
```

---

# 9. Step 4: DHCP Acknowledgment

The DHCP server receives the DHCP Request.

The server confirms the IP address assignment.

The server sends:

```text
DHCP ACK
```

The client receives the final network configuration.

Example:

```text
IP Address: 40.0.0.11
Subnet Mask: 255.255.255.0
Default Gateway: 40.0.0.10
DNS Server: 8.8.8.8
```

Now the client can communicate on the network.

---

# 11. DHCP Lease

A DHCP IP address is usually assigned for a limited period of time.

This period is called a **Lease Time**.

Example:

```text
IP Address:
40.0.0.11

Lease Time:
24 Hours
```

The client can use the IP address during the lease period.

Before the lease expires, the client can request renewal.

If the device leaves the network and the lease expires, the DHCP server can assign that IP address to another client.

---

# 12. DHCP Lease Renewal

Example:

```text
Client IP:
40.0.0.11

Lease Time:
24 Hours
```

Before the lease expires, the client contacts the DHCP server.

The client requests to continue using the same IP address.

If approved:

```text
Lease Renewed
```

The client continues using the assigned IP address.

---

# 13. DHCP Ports

DHCP uses the UDP protocol.

The port numbers are:

```text
DHCP Server → UDP Port 67
DHCP Client → UDP Port 68
```

Summary:

```text
Client → UDP 68
Server → UDP 67
```

---

# 14. DHCP Broadcast

A new DHCP client usually does not know:

- Its own IP address.
- The DHCP server address.
- The default gateway.

Therefore, the DHCP Discover message is normally sent as a broadcast.

Example:

```text
PC
 |
 | Broadcast DHCP Discover
 |
 ↓
Switch
 |
 ├── DHCP Server
 └── Other Devices
```

The DHCP server responds to the client.

---

# 15. DHCP Relay Agent

Routers normally do not forward broadcast traffic between different networks.

Therefore, if the DHCP server is located on another network, the client may need a DHCP Relay Agent.

Example:

```text
Client Network
40.0.0.0/24

PC
 |
Switch
 |
Router
 |
 |
Server Network
50.0.0.0/24
 |
DHCP Server
50.0.0.10
```

The router can forward DHCP requests to the DHCP server.

Cisco command:

```cisco
Router(config)#interface g0/0
Router(config-if)#ip helper-address 50.0.0.10
```

Here:

```text
50.0.0.10
```

is the DHCP server IP address.

The router acts as a DHCP Relay Agent.

---

# 16. Cisco Router as DHCP Server

A Cisco router can work as a DHCP server.

The basic process is:

1. Configure the router interface.
2. Exclude reserved IP addresses.
3. Create a DHCP pool.
4. Configure the network.
5. Configure the default gateway.
6. Configure the DNS server.
7. Configure clients to receive addresses using DHCP.

---

# 17. Verify DHCP Binding

To check which IP addresses are assigned:

```cisco
Router#show ip dhcp binding
```

Example:

```text
IP Address        Client
40.0.0.11         PC1
40.0.0.12         PC2
40.0.0.13         PC3
```

This command shows the DHCP bindings.

---

# 18. Verify DHCP Pool

Use:

```cisco
Router#show ip dhcp pool
```

This command shows:

- DHCP pool name.
- Network.
- Available addresses.
- Used addresses.
- Number of clients.

---

# 19. Common DHCP Problems

A DHCP client may fail to receive an IP address for several reasons.

Possible reasons include:

- DHCP server is not configured.
- Router interface is down.
- Wrong network address is configured.
- DHCP pool is incorrect.
- All available IP addresses are already used.
- Client is configured with static IP instead of DHCP.
- DHCP server is on another network without a relay agent.
- VLAN configuration is incorrect.
- Network connection is down.

---

# 20. DHCP Troubleshooting

Check interface status:

```cisco
show ip interface brief
```

Check DHCP pool:

```cisco
show ip dhcp pool
```

Check assigned addresses:

```cisco
show ip dhcp binding
```

Check DHCP statistics:

```cisco
show ip dhcp server statistics
```

Check running configuration:

```cisco
show running-config
```

Verify that:

- Router interface is up.
- Correct network is configured.
- Correct subnet mask is configured.
- Default gateway is correct.
- DHCP pool exists.
- Addresses are available.
- Client is configured for DHCP.

---

# 21. DHCP Security

An unauthorized DHCP server can create network problems.

A fake DHCP server may provide incorrect:

- IP addresses.
- Default gateways.
- DNS servers.

This can cause communication problems or redirect client traffic.

A Layer 2 security feature called **DHCP Snooping** can help protect the network.

---

# 22. DHCP Snooping

DHCP Snooping identifies switch ports as:

- Trusted ports.
- Untrusted ports.

Normally:

```text
Trusted Port
```

Connects to the legitimate DHCP server or authorized network device.

```text
Untrusted Port
```

Connects to normal clients.

DHCP Snooping helps protect against unauthorized DHCP servers.

Basic idea:

```text
Legitimate DHCP Server
        |
     Trusted Port
        |
      Switch
      /    \
Untrusted  Untrusted
  PC1        PC2
```

---

# 23. Important Points to Remember

- DHCP stands for Dynamic Host Configuration Protocol.
- DHCP automatically assigns network configuration.
- DHCP Server uses UDP port 67.
- DHCP Client uses UDP port 68.
- DHCP uses the DORA process.
- DORA means Discover, Offer, Request, and Acknowledgment.
- A DHCP Pool contains available IP addresses.
- Excluded addresses are not assigned to DHCP clients.
- DHCP can provide IP Address, Subnet Mask, Default Gateway, DNS Server, and other configuration.
- DHCP addresses are usually assigned using a lease.
- DHCP clients can renew their leases.
- Cisco routers can work as DHCP servers.
- `show ip dhcp binding` displays assigned addresses.
- `show ip dhcp pool` displays DHCP pool information.
- `ip helper-address` is used when the DHCP server is located on another network.
- DHCP Snooping helps protect against unauthorized DHCP servers.
- DHCP reduces manual configuration and configuration errors.

```

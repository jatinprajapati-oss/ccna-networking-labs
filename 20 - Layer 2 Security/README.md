# 🔐 Cisco Switch Port Security

## 📌 Project Overview

This project demonstrates **Switch Port Security** in Cisco Packet Tracer.

Port Security is a Layer 2 security feature used on a switch to control which devices are allowed to connect to a switch port.

In this topology, different PCs are connected to a Cisco switch. Port Security is configured on switch access ports to restrict unauthorized devices.

The project demonstrates two methods:

1. Dynamic MAC Address Learning
2. Static MAC Address Configuration

The main goal is to allow only authorized devices to use specific switch ports and block or restrict unauthorized devices.

---

# 📚 What is Port Security?

Port Security is a security feature available on Cisco switches.

Normally, any device can be connected to an active switch port. The switch will learn the MAC address of that device and allow communication.

This can create a security problem.

For example:

- An authorized PC is connected to a switch.
- Someone disconnects that PC.
- An unauthorized laptop is connected to the same switch port.
- Without Port Security, the new device may also get network access.

Port Security helps prevent this problem.

Port Security can restrict which MAC addresses are allowed on a switch port.

If an unauthorized device connects to the protected port, the switch can take an action based on the configured violation mode.

---

# 🎯 Purpose of Port Security

Port Security is used to:

- Allow only authorized devices.
- Restrict unauthorized devices.
- Limit the number of devices on a switch port.
- Control MAC addresses.
- Improve Layer 2 security.
- Prevent unauthorized network access.
- Reduce the risk of MAC address attacks.
- Protect unused or sensitive switch ports.
- Control which device can use a specific network connection.

---

# 🌐 How Port Security Works

Every network device has a MAC address.

A switch uses MAC addresses to forward Ethernet frames.

When Port Security is enabled on a switch port, the switch checks the MAC address of devices connected to that port.

The switch can:

1. Dynamically learn a MAC address.
2. Configure a MAC address manually.
3. Learn and save MAC addresses using sticky learning.
4. Limit the maximum number of allowed MAC addresses.

If the MAC address is authorized, communication is allowed.

If the MAC address is not authorized, a Port Security violation occurs.

---











# Port Security Configuration

The basic Port Security command is:

Switch(config-if)# switchport port-security

This enables Port Security on the selected switch port.

Example:

```text
Switch> enable
Switch# configure terminal
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```
Now Port Security is enabled on:
Fa0/1

# Maximum Number of MAC Addresses

By default, a Port Security-enabled port allows a limited number of MAC addresses.

We can manually configure the maximum number of allowed MAC addresses.

Example:
```text
Switch(config-if)# switchport port-security maximum 1
```
This means:

Only 1 MAC address is allowed on this port.

## 1️. Static MAC Address Port Security

Static MAC Address Port Security means we manually configure the MAC address that is allowed to connect to the switch port.

Only the configured MAC address is allowed.

Example:

PC0 MAC Address = 00D0.FF37.B7DC

We configure this MAC address on the switch port.

Configuration:

```text
Switch> enable
Switch# configure terminal
Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security mac-address 00D0.FF37.B7DC
```
The switch will allow:

00D0.FF37.B7DC

on that port.

If another device with a different MAC address tries to connect, it will create a Port Security violation depending on the configured violation mode.


## 2️. Sticky MAC Address Port Security

Sticky MAC is one of the easiest Port Security methods.

Instead of manually typing the MAC address, the switch automatically learns the MAC address of the connected device.

The learned MAC address becomes a Sticky Secure MAC Address.

Configuration:

```text
Switch> enable
Switch# configure terminal
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security mac-address sticky
```
The switch will learn the MAC address of the first connected device.


🔍 Verify Port Security

To check Port Security configuration for a specific interface:

```text
Switch# show port-security interface fa0/1
```
This command displays information such as:

```text
Port Security

Port Status

Violation Mode

Maximum MAC Addresses

Total MAC Addresses

Secure MAC Addresses

Security Violation Count
```

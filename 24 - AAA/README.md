# AAA (Authentication, Authorization and Accounting)

## Introduction

AAA stands for:

- **Authentication**
- **Authorization**
- **Accounting**

AAA is a security framework used to control and manage user access to network devices and services.

In a network, many users may try to access routers, switches, servers, wireless devices, VPNs, or other network resources. The administrator needs to know:

1. Who is trying to access the device?
2. Is the user really who they claim to be?
3. What is the user allowed to do after login?
4. What actions did the user perform?

AAA helps answer all these questions.

AAA is commonly used in enterprise networks to provide centralized and secure access control.

The complete flow is:

**User → Authentication → Authorization → Access to Resources → Accounting**

---

# 1. Authentication

### What is Authentication?

Authentication is the process of verifying the identity of a user.

In simple words:

```text
**Authentication answers the question: "Who are you?"**
```
Before allowing a user to access a router, switch, server, or network service, the system checks whether the user is genuine.

The most common authentication method is:
- Username
- Password

For example:

```text
Username: jatin
Password: 12345
```
---

# Authentication Methods

AAA can use different methods to verify a user.

### Local Authentication

Usernames and passwords are stored directly on the router or switch.

Example:

username jatin secret jatin123

The router stores the local user account.

When the user tries to log in, the router checks the local database.

Advantages
- Easy to configure.
- Does not require an external server.
- Useful for small networks.
- Works even if the network connection to an authentication server is unavailable.

Disadvantages
- Difficult to manage in a large network.
- User accounts must be created on multiple devices.

### Remote Authentication

User information is stored on a central AAA server.

Network devices communicate with the server to verify users.

Common protocols are:

RADIUS
TACACS+

Example:
```text
User → Router/Switch → AAA Server → Authentication Result
```
The user sends login credentials.

The network device sends the authentication request to the AAA server.

The AAA server checks the user information.

The server then sends:

Accept
or
Reject

Advantages
- Centralized user management.
- Easier for large networks.
- Users can be managed from one central location.
- Better security and control.

---
# Authorization

### What is Authorization?

Authorization decides what an authenticated user is allowed to do.

In simple words:
Authorization answers the question: "What are you allowed to do?"

Authentication happens first.
After the user is successfully authenticated, authorization decides the user's permissions.

For example, two users may successfully log in:

User 1: Network Administrator
User 2: Normal User

Both users are authenticated.

However, their permissions can be different.

The Network Administrator may be allowed to:

- Configure interfaces.
- Create VLANs.
- Change routing.
- Create users.
- etc

A normal user may only be allowed to:

- View basic information.
- Run limited commands.
- Check device status.

  Authorization controls these permissions.
---
### Authentication vs Authorization

Authentication

Authentication checks: Who are you?

- The device verifies the identity.

Authorization

Authorization checks: What are you allowed to do?

Example:

User jatin → Full administrator access
User user1 → Only monitoring access

Therefore:

Authentication = Identity verification

Authorization = Permission control

---
# Accounting

### What is Accounting?

Accounting records and tracks user activity.

In simple words:

Accounting answers the question: "What did the user do?"

Accounting can record information such as:

- Who logged in.
- Login time.
- Logout time.
- Commands used by the user.
- Configuration changes.
- Session duration.

Example:
```text
User: jatin
Login Time: 10:00 AM
Logout Time: 11:30 AM
Session Duration: 1 Hour 30 Minutes
```
The administrator can use these records for monitoring and auditing.

#### Why Accounting is Important

Accounting helps administrators:
```text
Track user activity.
Detect unauthorized actions.
Maintain security logs.
Troubleshoot problems.
Audit configuration changes.
Know when a user accessed a device.
```
For example, if someone changes a router configuration, accounting logs can help identify:

- Which user made the change?
- When was the change made?
- What command was used?
- AAA Complete Process

---
### AAA normally works in the following order:

Step 1: Authentication

The user tries to access a network device.

Example:
```text
Username: jatin
Password: jatin123
```
The device verifies the user's identity.

If authentication fails:
Access Denied

If authentication succeeds, the process continues.

Step 2: Authorization

After successful authentication, the system checks the user's permissions.

Example:
```text
User: jatin
Role: Administrator
Permission: Full Access
```

Step 3: Accounting

The system records the user's activity.

Example:
```text
User logged in.
Login time recorded.
Commands recorded.
Logout time recorded.
Session information stored.
Simple AAA Example
```

---
# AAA Components

A typical AAA environment contains three main components.

1. User - person
2. Network Access Device - Router, Switch, etc
3. AAA Server - The AAA server stores user information and processes authentication, authorization, and accounting requests.

Examples of AAA technologies include:

RADIUS
TACACS+

Two common protocols used for centralized AAA are:

RADIUS
TACACS+

### RADIUS

RADIUS stands for:

Remote Authentication Dial-In User Service

RADIUS is commonly used for network access authentication.

It is often used with:

- Wireless networks.
- VPN access.
- Network access control.
- ISP networks.

RADIUS combines authentication and authorization processes closely together.

#### RADIUS Characteristics

- Uses a client-server model.
- Provides centralized authentication.
- Supports authorization.
- Supports accounting.
- Commonly used for network access.
- Uses UDP.

Common RADIUS ports are:

Authentication: UDP 1812
Accounting: UDP 1813

------
### TACACS+

TACACS+ stands for:

Terminal Access Controller Access-Control System Plus

TACACS+ is commonly used for controlling administrative access to network devices.

It is especially useful for:

- Cisco routers.
- Cisco switches.
- Firewalls.
- Network device administration.

TACACS+ separates:

- Authentication
- Authorization
- Accounting

This provides detailed control over user permissions.

#### TACACS+ Characteristics

Important characteristics:

- Uses TCP.
- Uses port 49.
- Separates Authentication, Authorization, and Accounting.
- Provides detailed command authorization.
- Commonly used for device administration.
- Encrypts more of the communication than RADIUS.

-----

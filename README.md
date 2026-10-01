````markdown
# Cyber Shield – Network Defense

A secure campus network defense architecture built with Cisco Packet Tracer as part of the **Virtual Internship Program 2025 – Cyber Security Stream**.

The project focuses on analyzing, securing, and extending a college campus network across three security scenarios: network security assessment, secure hybrid remote access, and web access control.

---

## Project Overview

| Item | Details |
|---|---|
| Project | Cyber Shield: Defending the Network |
| Program | Virtual Internship Program 2025 |
| Stream | Cyber Security |
| Platform | Cisco Packet Tracer |
| Domain | Cybersecurity / Network Security |
| Status | Practical implementation completed |

---

## Objectives

- Analyze the college campus network and identify security risks.
- Implement network segmentation using VLANs.
- Apply ACL-based access control between trust zones.
- Protect management and server networks.
- Secure network-device administration using SSH.
- Implement switch port security.
- Provide secure remote faculty access using IPsec VPN.
- Implement DNS-based web filtering using a sinkhole simulation.
- Monitor network events using centralized Syslog.
- Identify security limitations and recommend production-level improvements.

---

# Part 1 – Network Security Assessment

Part 1 analyzes the campus network from an internal red-team perspective and identifies potential attack surfaces, trust boundaries, and security weaknesses.

## Implemented Security Controls

- VLAN-based network segmentation
- Inter-VLAN routing
- Extended ACLs
- Perimeter traffic filtering
- Guest network isolation
- Server network protection
- Management network isolation
- SSHv2 management
- Local AAA authentication
- VTY source restrictions
- Switch port security
- Centralized Syslog monitoring

## Network Zones

| VLAN | Zone | Network |
|---|---|---|
| VLAN 10 | Administration | 10.10.10.0/24 |
| VLAN 20 | Faculty | 10.10.20.0/24 |
| VLAN 30 | Students | 10.10.30.0/24 |
| VLAN 40 | Guest | 10.10.40.0/24 |
| VLAN 50 | Servers | 10.10.50.0/24 |
| VLAN 60 | Management | 10.10.60.0/24 |

## Security Assessment

The network was analyzed for:

- Trust-zone separation
- Lateral movement opportunities
- Internet-facing attack surfaces
- Wireless access points
- Server exposure
- Management-plane exposure
- Unauthorized access paths
- Authentication boundaries
- Web-access control weaknesses

Risk-based countermeasures were identified for the major attack surfaces.

---

# Part 2 – Secure Hybrid Access

Part 2 introduces secure remote access for faculty while preventing direct exposure of internal services to the Internet.

## Implemented

- Remote Faculty network
- Faculty home wireless network
- Dedicated VPN router
- ISP/WAN simulation
- Site-to-site IPsec VPN
- ISAKMP Phase 1
- IPsec transform set
- Crypto map
- VPN traffic ACLs
- NAT exemption for VPN traffic
- Remote Faculty access to approved internal services
- Management network isolation

## Remote Access Architecture

```text
Faculty Devices
      |
      v
   WRT300N
      |
      v
FACULTY-VPN-RTR
      |
      v
    ISP-R1
      |
      v
   EDGE-R1
      |
      v
   CORE-SW
      |
      v
Campus Internal Services
````

## VPN Verification

The IPsec VPN was tested in both directions.

Verified:

* Remote Faculty → Campus
* Campus → Remote Faculty
* Remote Faculty → Web Server
* Remote Faculty → Database Server
* Remote Faculty → File Server
* Remote Faculty → Authentication Server
* Remote Faculty → DNS Server
* Remote Faculty → Internet
* Remote Faculty → Management VLAN was blocked

IPsec encapsulation, encryption, decapsulation, and decryption were verified using Packet Tracer security-association information.

---

# Part 3 – Web Access Control

Part 3 implements a web-access control framework for students, faculty, and guests.

The design considers:

* User identity
* Content category
* Restricted destinations
* Network trust level
* Monitoring and logging
* Circumvention risks

## Implemented

* DNS-based filtering simulation
* DNS sinkhole mechanism
* Student web policy
* Faculty web policy
* Guest web policy
* Guest-to-server isolation
* Restricted destination blocking
* Internet access simulation
* Centralized Syslog monitoring

## DNS Filtering Model

```text
User Device
     |
     v
 DNS Server
     |
     +---- Allowed Destination
     |
     +---- Restricted Destination
                |
                v
           192.0.2.1
            Sinkhole
                |
                v
             ACL Block
```

The address `192.0.2.1` is used as a simulated sinkhole destination for restricted domains.

Example DNS records:

```text
blocked.example       -> 192.0.2.1
social-block.example  -> 192.0.2.1
```

This represents a DNS filtering/sinkhole simulation in Packet Tracer and is not intended to represent a production-grade category-aware DNS filtering service.

---

# Web Access Policy

| User Group | Internal Resources              | Internet | Restricted Destinations |
| ---------- | ------------------------------- | -------- | ----------------------- |
| Student    | Allowed according to ACL policy | Allowed  | Blocked                 |
| Faculty    | Allowed according to ACL policy | Allowed  | Blocked                 |
| Guest      | Restricted                      | Allowed  | Blocked                 |

Guest users are prevented from directly accessing protected internal servers while retaining access to the simulated Internet.

---

# Monitoring and Logging

A dedicated Syslog server is deployed in the Server VLAN.

**Syslog Server:** `10.10.50.60`

Network-device events were successfully forwarded to the centralized Syslog server.

Packet Tracer IOS does not support the ACL `log` keyword used for detailed ACL-match logging. Therefore, ACL-specific Syslog events are not claimed as implemented.

---

# Security Testing

The implementation was tested using Packet Tracer connectivity tests, browser tests, ACL counters, routing verification, NAT verification, and IPsec security-association information.

## Verified Controls

* Guest → Internal Server: **Blocked**
* Guest → Internet: **Allowed**
* Guest → Restricted Sinkhole: **Blocked**
* Remote Faculty → Internal Services: **Allowed**
* Remote Faculty → Management VLAN: **Blocked**
* Internet → Protected Internal Server: **Blocked**
* IPsec VPN: **Active**
* IPsec encryption/decryption: **Verified**
* Centralized Syslog: **Verified**
* SSHv2 management: **Implemented**
* Port Security: **Implemented**
* DNS filtering simulation: **Verified**

---

# Attack Surface

The main attack surfaces identified in the project include:

### 1. Internet / WAN Perimeter

The Internet-facing boundary is protected using perimeter ACL filtering.

### 2. Wireless Networks

Faculty and Guest wireless networks are separated into different trust zones.

### 3. Server Network

Servers are isolated in VLAN 50 and protected from unauthorized Guest access.

### 4. Management Network

The management VLAN is isolated from Student, Faculty, Guest, and Remote Faculty networks.

### 5. User VLANs

Administration, Faculty, Student, and Guest networks are separated using VLANs and ACL policies.

### 6. Remote Access

Remote Faculty access is provided through an IPsec VPN instead of exposing internal services directly to the Internet.

---

# Limitations

Some production-level controls could not be fully implemented because of Cisco Packet Tracer platform limitations.

## Time-Based Web Access

The project includes policy requirements based on:

* User group
* Access time
* Content category

However, the Packet Tracer IOS image used in this project does not support the required `time-range` functionality.

Therefore:

* Policy design: **Completed**
* Time-based enforcement: **Not implemented**
* Production recommendation: Use a next-generation firewall, secure web gateway, proxy, or DNS filtering platform supporting scheduled policies.

## RADIUS / Central AAA

Local AAA authentication is implemented and verified for SSH management.

RADIUS-based centralized authentication was explored but was not included as a verified implementation.

This is treated as a recommended production improvement rather than an implemented feature.

---

# Recommended Production Improvements

The Packet Tracer implementation provides a practical network-security baseline.

For a production environment, the architecture could be strengthened with:

* Next-generation firewall
* IDS/IPS
* WPA2/WPA3-Enterprise
* 802.1X / NAC
* Centralized RADIUS/AAA
* Multi-factor authentication
* Secure web gateway
* Layer 7 proxy/firewall
* Enterprise DNS filtering
* Endpoint Detection and Response (EDR)
* SIEM integration
* Dedicated DMZ
* More granular server access policies
* Scheduled web-access enforcement

---

# Technologies Used

* Cisco Packet Tracer
* Cisco IOS
* VLAN
* Inter-VLAN Routing
* Extended ACL
* Standard ACL
* SSHv2
* Local AAA
* Port Security
* NAT/PAT
* IPsec VPN
* ISAKMP
* DNS
* DNS Sinkhole Simulation
* Syslog

---

# Network Topology

The following diagram shows the complete campus network architecture, including the Internet/WAN perimeter, campus VLANs, server network, management network, wireless networks, and remote faculty IPsec VPN.

<img width="1040" height="604" alt="image" src="https://github.com/user-attachments/assets/fc567847-9d86-49ab-b7db-d6d60a92bb7f" />

---

# Cyber Shield – Network Defense

A secure campus network defense architecture designed and implemented using Cisco Packet Tracer.

The project focuses on network segmentation, access control, secure remote access, web-access control, network-device security, and centralized monitoring across a simulated college campus network.

---

## Project Overview

| Item | Details |
|---|---|
| Project | Cyber Shield – Network Defense |
| Platform | Cisco Packet Tracer |
| Domain | Cybersecurity / Network Security |
| Architecture | Campus Network + Remote Faculty Access |
| Network Model | Segmented Multi-VLAN Architecture |
| Remote Access | Site-to-Site IPsec VPN |
| Web Control | DNS Sinkhole Simulation + ACL Policies |
| Management | SSHv2 + Local AAA |
| Monitoring | Centralized Syslog |
| Status | Practical implementation completed |

---

## Objectives

- Analyze the security posture of a simulated college campus network.
- Identify network attack surfaces and trust boundaries.
- Implement network segmentation using VLANs.
- Control inter-network communication using ACLs.
- Protect server and management networks.
- Secure network-device administration using SSHv2.
- Implement switch port security.
- Provide secure remote faculty connectivity using IPsec VPN.
- Implement DNS-based web filtering using a sinkhole simulation.
- Isolate Guest users from protected internal resources.
- Monitor network-device events using centralized Syslog.
- Test implemented security controls using Packet Tracer verification methods.
- Identify platform limitations and recommend production-level improvements.

---

# 1. Network Security Architecture

The campus network is divided into separate security zones to reduce lateral movement and control communication between trusted and untrusted networks.

## Network Zones

| VLAN | Zone | Network | Purpose |
|---|---|---|---|
| VLAN 10 | Administration | 10.10.10.0/24 | Administrative users |
| VLAN 20 | Faculty | 10.10.20.0/24 | Faculty users |
| VLAN 30 | Students | 10.10.30.0/24 | Student users |
| VLAN 40 | Guest | 10.10.40.0/24 | Untrusted guest devices |
| VLAN 50 | Servers | 10.10.50.0/24 | Internal services |
| VLAN 60 | Management | 10.10.60.0/24 | Network administration |

## Implemented Security Controls

- VLAN-based network segmentation
- Inter-VLAN routing
- Extended ACLs
- Standard ACLs
- Guest network isolation
- Server network protection
- Management network isolation
- Perimeter ACL filtering
- SSHv2 management
- Local AAA authentication
- VTY source restrictions
- Switch port security
- NAT/PAT
- Centralized Syslog monitoring

---

# 2. Network Topology

The following architecture represents the implemented campus network, including the WAN perimeter, segmented campus VLANs, server infrastructure, management network, wireless networks, and remote faculty IPsec VPN.

<img width="1255" height="701" alt="image" src="https://github.com/user-attachments/assets/86078458-d228-4a01-b2e8-8ae42d4b32a4" />

## Topology Components

| Component | Type | Purpose |
|---|---|---|
| ISP-R1 | Router | Internet/WAN connectivity and remote-access network |
| EDGE-R1 | Router | Campus perimeter routing, filtering, NAT/PAT, and IPsec VPN |
| CORE-SW | Multilayer Switch | Inter-VLAN routing and central security policy enforcement |
| SW-ADMIN | Access Switch | Administration VLAN connectivity |
| SW-FACULTY | Access Switch | Faculty VLAN connectivity |
| SW-STUDENT | Access Switch | Student VLAN connectivity |
| SW-GUEST | Access Switch | Guest VLAN connectivity |
| SW-SERVER | Access Switch | Server VLAN connectivity |
| FACULTY-VPN-RTR | Router | Remote faculty VPN gateway |
| FACULTY-HOME-RTR / WRT300N | Wireless Router | Remote faculty home wireless network |
| AP-FACULTY | Access Point | Faculty wireless access |
| AP-GUEST | Access Point | Guest wireless access |
| INTERNET-SRV | Server | Simulated Internet service |
| SRV-DNS | Server | Internal DNS and DNS sinkhole simulation |
| SRV-WEB | Server | Internal web service |
| SRV-DB | Server | Database service |
| SRV-FILE | Server | File service |
| SRV-AUTH | Server | Authentication service |
| SRV-SYSLOG | Server | Centralized Syslog monitoring |
| IT-ADMIN-PC | PC | Network management workstation |
| ADMIN-PC1–4 | PCs | Administration users |
| FACULTY-PC1–4 | PCs | Faculty users |
| STUDENT-PC1–8 | PCs | Student users |
| GUEST-LAPTOP1–2 | Laptops | Guest users |
| FACULTY-1–2 | Laptops | Remote faculty users |

---

# 3. Part 1 – Network Security Assessment

The network was analyzed from an internal security-assessment perspective to identify attack surfaces, trust boundaries, unauthorized access paths, and opportunities for lateral movement.

## Security Assessment Areas

- Trust-zone separation
- Inter-VLAN communication
- Internet-facing attack surface
- Wireless networks
- Server exposure
- Management-plane exposure
- Unauthorized access paths
- Authentication boundaries
- Guest network isolation
- Web-access control

## Security Boundaries

### Guest Network

Guest users are isolated from protected internal networks.

Implemented controls include:

- Guest VLAN
- DNS access to the internal DNS server
- Guest-to-server isolation
- Guest-to-management isolation
- Guest web-access restrictions
- Internet access simulation

### Server Network

Servers are placed in VLAN 50.

Guest access to the Server VLAN is blocked except for required DNS communication.

### Management Network

The Management VLAN is separated from:

- Student network
- Faculty network
- Guest network
- Remote Faculty network

Network-device management is restricted using SSHv2 and source-based VTY access control.

---

# 4. Access Control

ACLs are used to enforce communication policies between security zones.

## Guest Policy

Guest traffic is permitted to:

- Access the DNS server for name resolution
- Communicate within the Guest network
- Access simulated Internet services

Guest traffic is denied access to protected campus networks and Server VLAN resources except for the required DNS service.

## Server Policy

Guest traffic to the Server VLAN is denied except for DNS communication with:

```text
SRV-DNS
10.10.50.10
```

## Management Policy

The Management VLAN is protected from:

```text
Student Network
Faculty Network
Guest Network
Remote Faculty Network
```

This prevents unauthorized users from directly accessing the network-management workstation.

---

# 5. Secure Network Device Management

Network-device administration is secured using SSHv2.

## Implemented Controls

- SSH version 2
- Local user authentication
- Local AAA
- VTY access restrictions
- RSA key generation
- Management VLAN restriction
- Privileged EXEC protection

Management access is restricted to the Management network.

This reduces exposure of the network-device management plane to normal user networks.

---

# 6. Switch Port Security

Port security was implemented on user-facing switch ports.

## Implemented Configuration

- Maximum MAC addresses per port
- Sticky MAC learning
- Violation mode: Restrict

Port security was applied to:

- Administration access ports
- Faculty access ports
- Student access ports
- Guest access ports
- Server access ports

This provides protection against unauthorized devices being connected to protected access ports.

---

# 7. Part 2 – Secure Remote Faculty Access

Remote faculty access is provided through a simulated site-to-site IPsec VPN.

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
```

## Implemented VPN Components

- Dedicated remote faculty network
- Faculty home wireless network
- Dedicated VPN router
- ISP/WAN simulation
- ISAKMP Phase 1
- IPsec transform set
- Crypto map
- VPN traffic ACLs
- NAT exemption for VPN traffic
- Remote faculty access to approved internal services
- Management VLAN isolation

## VPN Parameters

The VPN uses:

- Pre-shared-key authentication
- AES encryption
- SHA hashing
- Diffie-Hellman Group 2
- IPsec ESP
- AES/SHA transform set

---

# 8. VPN Security Policy

Remote Faculty users are allowed to access required internal services through the encrypted VPN tunnel.

Verified access includes:

- Web Server
- Database Server
- File Server
- Authentication Server
- DNS Server
- Simulated Internet access

Remote Faculty access to the Management VLAN is blocked.

This separates normal application access from network-management access.

---

# 9. VPN Verification

The VPN was verified using Packet Tracer connectivity testing and IPsec security-association information.

## Verified

- Remote Faculty → Campus
- Campus → Remote Faculty
- Remote Faculty → Web Server
- Remote Faculty → Database Server
- Remote Faculty → File Server
- Remote Faculty → Authentication Server
- Remote Faculty → DNS Server
- Remote Faculty → Internet
- Remote Faculty → Management VLAN blocked

IPsec encapsulation, encryption, decapsulation, and decryption were verified using the router security-association information.

---

# 10. Part 3 – Web Access Control

A DNS-based web-access control simulation was implemented for Student, Faculty, and Guest networks.

The implementation uses a DNS sinkhole model combined with ACL-based destination blocking.

## Web Access Model

| User Group | Internal Resources | Internet | Restricted Destinations |
|---|---|---|---|
| Student | Allowed according to ACL policy | Allowed | Blocked |
| Faculty | Allowed according to ACL policy | Allowed | Blocked |
| Guest | Restricted | Allowed | Blocked |

---

# 11. DNS Sinkhole Simulation

The internal DNS server is:

```text
SRV-DNS
10.10.50.10
```

Restricted domains are mapped to a simulated sinkhole address:

```text
192.0.2.1
```

Example records:

```text
blocked.example       -> 192.0.2.1
social-block.example  -> 192.0.2.1
```

Traffic destined for the simulated sinkhole is then blocked using ACL policies.

## Important Scope

This is a **DNS filtering and sinkhole simulation** implemented in Cisco Packet Tracer.

It is not a production-grade category-aware DNS filtering service.

---

# 12. Web Access Enforcement

Separate policies were implemented for:

- Students
- Faculty
- Guests

## Student

Restricted destinations are blocked while permitted internal and Internet destinations remain accessible.

## Faculty

Restricted destinations are blocked while permitted internal and Internet destinations remain accessible.

## Guest

Guests are prevented from accessing protected internal servers while retaining access to the simulated Internet.

---

# 13. Monitoring and Logging

A dedicated Syslog server is deployed in the Server VLAN.

```text
SRV-SYSLOG
10.10.50.60
```

Network-device logging is configured to forward events to the centralized Syslog server.

## Monitoring Scope

The implementation verifies:

- Network-device event forwarding
- Centralized Syslog availability
- Router/switch logging configuration

### Platform Limitation

The Packet Tracer IOS image does not support the ACL `log` keyword used for detailed ACL-match logging.

Therefore, ACL-specific Syslog events are **not claimed as implemented**.

---

# 14. Security Testing

The network was tested using:

- Packet Tracer Simple PDU
- Ping/connectivity tests
- Browser tests
- ACL counters
- Routing verification
- NAT verification
- IPsec security-association information
- SSH verification
- Port-security verification
- DNS resolution testing

## Verified Security Controls

| Test | Result |
|---|---|
| Guest → Internal Server | Blocked |
| Guest → Internet | Allowed |
| Guest → Restricted Sinkhole | Blocked |
| Remote Faculty → Internal Services | Allowed |
| Remote Faculty → Management VLAN | Blocked |
| Internet → Protected Internal Server | Blocked |
| IPsec VPN | Active |
| IPsec Encryption/Decryption | Verified |
| Centralized Syslog | Verified |
| SSHv2 Management | Implemented |
| Port Security | Implemented |
| DNS Sinkhole Simulation | Verified |

---

# 15. Attack Surface Analysis

The major attack surfaces identified in the architecture are:

## 1. Internet / WAN Perimeter

The Internet-facing boundary is protected using perimeter ACL filtering.

### Residual Risk

A production deployment would require stronger perimeter controls such as:

- Next-generation firewall
- IDS/IPS
- Application-layer inspection

---

## 2. Wireless Networks

Faculty and Guest wireless networks are separated into different trust zones.

### Residual Risk

Production wireless security should use:

- WPA2/WPA3-Enterprise
- 802.1X
- Centralized authentication
- Rogue access-point detection
- Client isolation where appropriate

---

## 3. Server Network

Servers are isolated in VLAN 50.

Guest access to protected server resources is restricted.

### Residual Risk

A production environment could further improve isolation using:

- Dedicated DMZ
- Server-specific ACLs
- Next-generation firewall
- Application-layer controls

---

## 4. Management Network

The Management VLAN is isolated from normal user networks.

### Residual Risk

Production environments should consider:

- Dedicated management firewall
- Centralized AAA
- Multi-factor authentication
- Out-of-band management

---

## 5. User VLANs

Administration, Faculty, Student, and Guest networks are separated using VLANs and ACL policies.

### Residual Risk

Production environments could add:

- Network Access Control
- 802.1X
- Endpoint Detection and Response
- More granular application-level policies

---

## 6. Remote Access

Remote Faculty access is provided through an IPsec VPN rather than directly exposing internal services to the Internet.

### Residual Risk

Production remote access should consider:

- MFA
- Centralized identity management
- Device posture validation
- Identity-aware access controls

---

# 16. Limitations

Some production-level controls could not be fully implemented because of Cisco Packet Tracer platform limitations.

## Time-Based Web Access

The intended policy model considers:

- User group
- Access time
- Content category

However, the Packet Tracer IOS image used in this project does not support the required `time-range` functionality.

Therefore:

| Capability | Status |
|---|---|
| Policy design | Completed |
| Time-based enforcement | Not implemented |
| Production solution | NGFW / Secure Web Gateway / Scheduled DNS Filtering |

The time-based requirement is therefore documented as a platform limitation rather than being falsely represented as implemented.

---

## Centralized AAA / RADIUS

Local AAA authentication is implemented and verified for SSH management.

RADIUS-based centralized authentication was explored but was not included as a verified implementation.

For production deployment, centralized AAA with MFA is recommended.

---

# 17. Recommended Production Improvements

The Packet Tracer implementation provides a practical network-security baseline.

For a production environment, the architecture could be strengthened with:

- Next-generation firewall
- IDS/IPS
- WPA2/WPA3-Enterprise
- 802.1X / NAC
- Centralized RADIUS/AAA
- Multi-factor authentication
- Secure Web Gateway
- Layer 7 proxy/firewall
- Enterprise DNS filtering
- Endpoint Detection and Response (EDR)
- SIEM integration
- Dedicated DMZ
- More granular server access policies
- Scheduled web-access enforcement
- Identity-aware remote access

---

# 18. Technologies Used

- Cisco Packet Tracer
- Cisco IOS
- VLAN
- Inter-VLAN Routing
- Extended ACL
- Standard ACL
- SSHv2
- Local AAA
- Port Security
- NAT/PAT
- IPsec VPN
- ISAKMP
- DNS
- DNS Sinkhole Simulation
- Syslog

---


# 19. Key Security Outcomes

The implemented architecture demonstrates:

- Segmentation of campus users into separate trust zones.
- Controlled communication between VLANs.
- Isolation of Guest users from protected internal resources.
- Protection of the Management VLAN.
- Secure network-device administration using SSHv2.
- Access-port protection using port security.
- Encrypted remote faculty connectivity using IPsec VPN.
- DNS sinkhole-based web filtering simulation.
- Centralized network-device logging using Syslog.
- Verification of security controls through practical network testing.
- Identification of limitations and production-level security improvements.

---

## Conclusion

Cyber Shield demonstrates a segmented and security-focused campus network architecture implemented in Cisco Packet Tracer.

The project combines network segmentation, ACL-based access control, secure device administration, port security, IPsec VPN remote access, DNS sinkhole simulation, and centralized Syslog monitoring.

The design also documents the limitations of a Packet Tracer-based environment and identifies additional controls required to transition the architecture toward a production-grade enterprise network.

---

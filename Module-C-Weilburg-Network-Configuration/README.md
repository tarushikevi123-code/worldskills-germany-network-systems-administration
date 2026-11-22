# Module C – Weilburg Enterprise Network Configuration

## 1. Project Overview

This project documents the implementation of the Weilburg enterprise network from the WorldSkills Germany 2025 IT Network Systems Administration practical task.

The topology uses two Layer 3 access switches, one Layer 3 core switch and two WAN routers. The implementation focuses on resilient switching, dynamic routing, centralized network services, WAN connectivity and network security.

**Platform:** Cisco Packet Tracer

## 2. Project Objectives

- Configure VLAN segmentation for HR, Sales, Engineering, WLAN and Server networks.
- Configure Layer 3 switching and inter-VLAN gateway services.
- Implement redundant access-to-core uplinks using EtherChannel.
- Configure Rapid PVST+ and define primary and secondary STP roots.
- Implement HSRP gateway redundancy and load sharing across the user VLANs.
- Configure OSPF across the Weilburg network and both WAN routers.
- Advertise default routes from the WAN routers into the internal OSPF domain.
- Provide centralized DHCP and DNS services, including DHCP relay.
- Configure NAT/PAT for access to the simulated external network.
- Secure device management using local authentication and SSH version 2.
- Restrict remote management access using a standard ACL.
- Apply Port Security on access ports.
- Filter access to the Intranet server using an extended ACL.
- Verify the network through routing, DNS, ping and web browser tests.

## 3. Network Architecture

The network consists of five Cisco devices:

### Switching Layer

- **Weil_A1 – Access Switch 1:** Layer 3 access switch. Configured as the preferred STP root for VLANs 2, 4, 6 and 8. HSRP active gateway for VLANs 2 and 4.
- **Weil_A2 – Access Switch 2:** Layer 3 access switch. Configured as the secondary STP root for VLANs 2, 4, 6 and 8. HSRP active gateway for VLANs 6 and 8.
- **Weil_C1 – Core Switch:** Central Layer 3 switch connecting both access switches to the WAN routers. Provides the Server VLAN interface and participates in OSPF.

### WAN Layer

- **Weil_W1 – WAN Router 1:** Connects the core network to the ISP and corporate WAN. Performs OSPF, default-route advertisement, NAT/PAT and Intranet ACL filtering.
- **Weil_W2 – WAN Router 2:** Provides a second WAN path and performs the corresponding OSPF, default-route advertisement, NAT/PAT and Intranet ACL functions.

## 4. VLAN and IPv4 Addressing Plan

| VLAN | Department / Purpose | Network | HSRP Virtual IP |
|---:|---|---|---|
| 2 | HR | `10.10.2.0/27` | `10.10.2.30` |
| 4 | Sales | `10.10.4.0/28` | `10.10.4.14` |
| 6 | Engineering | `10.10.6.0/26` | `10.10.6.62` |
| 8 | WLAN | `10.10.8.0/29` | `10.10.8.6` |
| 130 | Servers | `10.10.130.0/29` | Core SVI: `10.10.130.6` |

### Centralized Services

| Service | IPv4 Address |
|---|---|
| DHCP Server | `10.10.130.2` |
| DNS Server | `10.10.130.3` |
| Intranet Server | `192.168.66.200` |
| Simulated External Web Server | `200.1.1.100` |

### Routed Transit Networks

| Connection | Network |
|---|---|
| Weil_A1 – Weil_C1 | `10.11.1.0/30` |
| Weil_A2 – Weil_C1 | `10.11.2.0/30` |
| Weil_C1 – Weil_W1 | `10.11.3.0/30` |
| Weil_C1 – Weil_W2 | `10.11.4.0/30` |
| Weil_W1 – ISP | `195.198.11.0/30` |
| Weil_W2 – ISP | `195.198.11.4/30` |
| Weil_W1 – Corporate WAN | `172.20.20.0/30` |
| Weil_W2 – Corporate WAN | `172.20.20.4/30` |

## 5. Implementation

### 5.1 VLANs and Layer 3 Switching

Created the required VLANs and configured the access-switch SVIs using the assigned addressing plan. The HSRP virtual addresses provide the default gateway addresses for the user VLANs.

### 5.2 EtherChannel

Configured aggregated uplinks between the access switches and the core switch to provide logical bundled links and redundant physical paths.

### 5.3 Spanning Tree Protocol

Configured Rapid PVST+ on both access switches. Weil_A1 was selected as the primary root and Weil_A2 as the secondary root for VLANs 2, 4, 6 and 8. PortFast and BPDU Guard were configured on access ports in accordance with the task requirements.

### 5.4 HSRP

Configured HSRP version 1 with group numbers corresponding to VLAN IDs. The intended active gateway distribution is:

- Weil_A1: VLAN 2 and VLAN 4.
- Weil_A2: VLAN 6 and VLAN 8.

Preemption and uplink tracking were also configured. Final priority and tracking compliance should be checked against the official task requirements before treating the HSRP section as fully signed off.

### 5.5 OSPF Dynamic Routing

Configured OSPF Process 1 across the access, core and WAN devices. The internal Weilburg routing domain uses Area 0, while the corporate-facing WAN links use Area 2. The WAN routers originate their default routes into the OSPF domain.

The configured router IDs are:

| Device | OSPF Router ID |
|---|---|
| Weil_A1 | `1.1.1.1` |
| Weil_A2 | `2.2.2.2` |
| Weil_C1 | `3.3.3.3` |
| Weil_W1 | `4.4.4.4` |
| Weil_W2 | `5.5.5.5` |

### 5.6 DHCP and DNS

The DHCP server is located in VLAN 130 at `10.10.130.2`. The DNS server is located at `10.10.130.3`. DHCP relay was configured on the user VLAN gateway interfaces to forward DHCP requests to the centralized server.

### 5.7 NAT/PAT

Configured a named standard ACL `vlans` to select internal source networks for translation. PAT overload uses each WAN router's ISP-facing Serial0/1/0 interface.

### 5.8 Device Management Security

Configured local user authentication, enable secret, password encryption, the domain name `skill39.org`, SSH version 2, console local authentication and an MOTD banner. VTY lines 0–15 use local authentication and accept SSH only. Standard ACL 1 restricts remote management sources to the Server VLAN subnet `10.10.130.0/29`.

Credential values are intentionally not reproduced in this public documentation.

### 5.9 Port Security

Port Security was configured on the access switches with a one-MAC limit for normal access interfaces and a 50-MAC limit for the designated WLAN access-point interface. The configured violation action is Protect. The AP interface assignment on both access switches remains subject to final verification against the physical topology and task specification.

### 5.10 Intranet Access Control

A named extended ACL `intranet` was configured on both WAN routers and applied inbound on GigabitEthernet0/0/0.

| Traffic to `192.168.66.200` | Policy |
|---|---|
| TCP port 443 – HTTPS | Permit |
| TCP port 80 – HTTP | Deny |
| Other IP traffic | Permit |

## 6. Verification and Testing

The following tests were performed and reported from a Packet Tracer client:

### DNS Resolution

Command:

```text
nslookup www.google.de
```

Observed result: DNS server `10.10.130.3` resolved `www.google.de` to `200.1.1.100`.

### External Server Reachability

Commands:

```text
ping www.google.de
ping 200.1.1.100
```

Observed result: both tests returned 4 replies from 4 packets sent (0% packet loss).

### Intranet Web Access

- HTTPS request to `https://192.168.66.200` succeeded.
- HTTP request to `http://192.168.66.200` was blocked.

Additional routing, redundancy, DHCP, SSH and Port Security checks are listed in the repository's validation matrix. See [Documentation](documentation/README.md).

## 7. Configuration Files

Device configuration exports are stored in the `configurations/` directory:

- `Weil_A1.txt`
- `Weil_A2.txt`
- `Weil_C1.txt`
- `Weil_W1.txt`
- `Weil_W2.txt`

## 8. Project Files

- [Cisco Packet Tracer working topology](topology/Module%20C%20%E2%80%93%20Networking.pka).
- Five device configuration exports in `configurations/` (to be added).
- Network addressing and topology documentation.
- Verification screenshots and test evidence.
- Configuration and troubleshooting notes.

## 9. Technologies and Tools

- Cisco Packet Tracer
- Cisco IOS
- VLANs
- Layer 3 Switching
- EtherChannel
- Rapid PVST+
- HSRP
- OSPF
- DHCP Relay
- DNS
- NAT/PAT
- SSH
- Standard and Extended ACLs
- Port Security

## 10. Project Status

**Implementation:** Configured and tested in Cisco Packet Tracer.

**Verified:** DNS name resolution, ICMP reachability to the simulated external web server, and Intranet HTTPS/HTTP ACL behavior.

**Pending:** Final HSRP priority/tracking audit, AP-port compliance verification, complete DHCP and SSH access tests, and inclusion of sanitized device configuration exports and screenshots.

---

**Project:** WorldSkills Germany 2025 – IT Network Systems Administration  
**Module:** C – Network Configuration  
**Network:** Weilburg Enterprise Network  
**Platform:** Cisco Packet Tracer

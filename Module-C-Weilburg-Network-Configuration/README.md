# Module C – Weilburg Enterprise Network

A Cisco Packet Tracer implementation of a redundant enterprise branch network, based on the **WorldSkills Germany 2025 – IT Network Systems Administration, Module C** task.

> Independent educational portfolio work. This repository is not affiliated with or endorsed by WorldSkills Germany or the competition organizers.

**Platform:** Cisco Packet Tracer  
**Network domain:** Routing & Switching · Enterprise Networking · Network Security  
**Devices:** 2 Layer 3 access switches · 1 Layer 3 core switch · 2 WAN routers

---

## Network Topology

The diagram shows the Weilburg branch, its internal OSPF Area 0, dual WAN connectivity to the Corporate and ISP routers, the Intranet server, simulated Internet endpoint, user VLANs and centralized DHCP/DNS services.

 · <img width="955" height="795" alt="image" src="https://github.com/user-attachments/assets/5415b89e-0ea2-4e71-97fb-5d0af8b1ff6a" />


## Project Overview

The Weilburg site is designed around a redundant Layer 3 campus network. Two multilayer access switches provide the user VLAN gateways and connect to a central Layer 3 core switch. The core connects to two WAN routers, which provide separate paths towards the Corporate WAN and the ISP.

The implementation covers:

- VLAN segmentation for HR, Sales, Engineering, WLAN and Servers.
- Inter-VLAN routing using Layer 3 switch virtual interfaces (SVIs).
- Layer 2 and Layer 3 EtherChannel using LACP.
- Rapid Spanning Tree design and root-bridge placement.
- HSRP default-gateway redundancy and load sharing.
- OSPF dynamic routing across the branch and WAN edge.
- Centralized DHCP and DNS services with DHCP relay.
- NAT/PAT for access to the simulated external network.
- SSH version 2, local authentication and management access control.
- Port Security on access-switch interfaces.
- Extended ACL policy for HTTPS/HTTP access to the Intranet server.

## Network Architecture

### Device roles

| Device | Layer / role | Main function |
|---|---|---|
| **Weil_A1** | Layer 3 access switch | User VLAN gateways; intended HSRP active for VLANs 2 and 4; primary STP root |
| **Weil_A2** | Layer 3 access switch | User VLAN gateways; intended HSRP active for VLANs 6 and 8; secondary STP root |
| **Weil_C1** | Layer 3 core switch | Aggregates both access switches and both WAN routers; provides the Server VLAN SVI |
| **Weil_W1** | WAN router 1 | Corporate and ISP connectivity, OSPF, default routing, NAT/PAT and Intranet ACL |
| **Weil_W2** | WAN router 2 | Secondary Corporate and ISP path, OSPF, default routing, NAT/PAT and Intranet ACL |

### Logical design

**Access layer:** Weil_A1 and Weil_A2 provide access connectivity for wired users and wireless access points. User VLANs are carried between the access switches over an L2 EtherChannel. Each access switch has an L3 EtherChannel uplink to Weil_C1.

**Core layer:** Weil_C1 provides the central routed point between the access layer and the WAN routers. The DHCP and DNS servers are connected to the Server VLAN (VLAN 130).

**WAN edge:** Weil_W1 and Weil_W2 connect the branch to both Corporate and ISP networks. The corporate-facing links are assigned to OSPF Area 2; the internal Weilburg routing domain uses Area 0.

## VLAN and IPv4 Addressing

| VLAN | Name / purpose | Network | Default gateway |
|---:|---|---|---|
| 2 | HR | `10.10.2.0/27` | HSRP VIP `10.10.2.30` |
| 4 | SALES | `10.10.4.0/28` | HSRP VIP `10.10.4.14` |
| 6 | ENGINEERING | `10.10.6.0/26` | HSRP VIP `10.10.6.62` |
| 8 | WLAN | `10.10.8.0/29` | HSRP VIP `10.10.8.6` |
| 130 | SERVER | `10.10.130.0/29` | Core SVI `10.10.130.6` |

### Access-switch port allocation

| Interfaces on A1 and A2 | VLAN | Purpose |
|---|---:|---|
| Gi1/0/3–8 | 2 | HR |
| Gi1/0/9–16 | 4 | Sales |
| Gi1/0/17–20 | 6 | Engineering |
| Gi1/0/21–22 | 8 | WLAN / access points |

On Weil_C1, Gi1/0/3–20 are intended for Server VLAN 130 endpoints. Check the live VLAN and interface state in Packet Tracer before final sign-off.

### Centralized services and application endpoints

| Service / endpoint | IPv4 address |
|---|---|
| DHCP server | `10.10.130.2` |
| DNS server | `10.10.130.3` |
| Intranet server | `192.168.66.200` |
| Simulated external web endpoint | `200.1.1.100` (`www.google.de` in the observed DNS test) |

## Routed Links and WAN Addressing

### Internal routed links

| Connection | Network | Weilburg endpoint addresses |
|---|---|---|
| Weil_A1 – Weil_C1 | `10.11.1.0/30` | A1: `10.11.1.2` · C1: `10.11.1.1` |
| Weil_A2 – Weil_C1 | `10.11.2.0/30` | A2: `10.11.2.2` · C1: `10.11.2.1` |
| Weil_C1 – Weil_W1 | `10.11.3.0/30` | C1: `10.11.3.1` · W1: `10.11.3.2` |
| Weil_C1 – Weil_W2 | `10.11.4.0/30` | C1: `10.11.4.1` · W2: `10.11.4.2` |

### Corporate and ISP links

| Connection | Network | Weilburg endpoint | Remote endpoint |
|---|---|---|---|
| W1 – Corporate | `172.20.20.0/30` | W1: `172.20.20.1` | Corporate: `172.20.20.2` |
| W2 – Corporate | `172.20.20.4/30` | W2: `172.20.20.5` | Corporate: `172.20.20.6` |
| W1 – ISP | `195.198.11.0/30` | W1: `195.198.11.1` | ISP: `195.198.11.2` |
| W2 – ISP | `195.198.11.4/30` | W2: `195.198.11.5` | ISP: `195.198.11.6` |

### Loopback management addresses

| Device | Loopback1 |
|---|---|
| Weil_C1 | `10.10.111.1/32` |
| Weil_A1 | `10.10.111.2/32` |
| Weil_A2 | `10.10.111.3/32` |
| Weil_W1 | `10.10.111.4/32` |
| Weil_W2 | `10.10.111.5/32` |

## Routing and Redundancy Design

### OSPF

OSPF Process 1 is used for dynamic routing. The intended router IDs and area assignments are:

| Device | Router ID | Area assignment |
|---|---|---|
| Weil_A1 | `1.1.1.1` | Area 0 |
| Weil_A2 | `2.2.2.2` | Area 0 |
| Weil_C1 | `3.3.3.3` | Area 0 |
| Weil_W1 | `4.4.4.4` | Area 0 internally; Corporate link in Area 2 |
| Weil_W2 | `5.5.5.5` | Area 0 internally; Corporate link in Area 2 |

The WAN routers use next-hop static default routes towards the ISP and are configured to originate the default route into OSPF.

### HSRP

HSRP version 1 provides a virtual default gateway for each user VLAN. The intended active-gateway distribution is:

| VLAN | Virtual IP | Intended active switch | Intended standby switch |
|---:|---|---|---|
| 2 | `10.10.2.30` | Weil_A1 | Weil_A2 |
| 4 | `10.10.4.14` | Weil_A1 | Weil_A2 |
| 6 | `10.10.6.62` | Weil_A2 | Weil_A1 |
| 8 | `10.10.8.6` | Weil_A2 | Weil_A1 |

The task specifies priority 105 on the preferred active switch, priority 100 on the standby switch, preemption, and tracking of the Layer 3 port-channel uplink.

### EtherChannel and Spanning Tree

- The A1–A2 inter-switch link is an L2 EtherChannel using LACP, with native VLAN 10 and only VLANs 2, 4, 6 and 8 allowed.
- A1 and A2 each have an L3 EtherChannel uplink to C1, using LACP.
- The required switching standard is Rapid PVST+ (802.1w), with A1 as primary root and A2 as secondary root.
- Access-port PortFast and BPDU Guard are required on the individual end-device interfaces.

## Network Services and Security

### DHCP and DNS

DHCP and DNS are centralized in VLAN 130. DHCP relay on the user VLAN gateway interfaces forwards client requests to `10.10.130.2`. DNS clients use `10.10.130.3`.

### NAT/PAT

The named standard ACL `vlans` selects the internal subnets for translation. PAT overload uses the ISP-facing Serial0/1/0 interface address on each WAN router.

The W2 VLAN 2 NAT wildcard was corrected from `0.0.0.3` to `0.0.0.31` after review. The updated device record reflects this correction; include a current `show access-lists vlans` output in the evidence set for final verification.

### Device management

The device configurations use local authentication, SSH version 2, SSH-only VTY transport, a management-source ACL, password encryption and an MOTD warning banner. Credential values and password hashes are intentionally excluded from the public configuration records.

### Intranet access policy

The extended ACL `intranet` is configured on both WAN routers and applied inbound on the site-facing GigabitEthernet0/0/0 interface.

| Traffic to `192.168.66.200` | Policy |
|---|---|
| TCP/443 – HTTPS | Permit |
| TCP/80 – HTTP | Deny |
| Other IP traffic | Permit |

## Verification Results

The following tests have been reported from the Packet Tracer environment:

| Test | Observed result |
|---|---|
| DNS lookup for `www.google.de` | DNS server `10.10.130.3` returned `200.1.1.100` |
| Ping to `www.google.de` | 4/4 replies reported |
| Ping to `200.1.1.100` | 4/4 replies reported |
| HTTPS to `https://192.168.66.200` | Succeeded |
| HTTP to `http://192.168.66.200` | Blocked |

These tests demonstrate the observed behavior for the tested client and simulated endpoints. They do not prove reachability to arbitrary public Internet destinations or complete compliance with every task requirement.

### Items still requiring final evidence

The project documentation keeps configuration review separate from operational verification. The following items need a final command output or repeatable test before full sign-off:

- W1's Corporate OSPF neighbor was last reported in EXSTART, while W1–C1 was FULL. W2's latest shared neighbor output showed both C1 and Corporate neighbors FULL. Recheck both WAN routers and capture the final state.
- The captured C1 configuration showed `spanning-tree mode pvst`; the task requires 802.1w / Rapid PVST+.
- The supplied A1/A2 configuration records showed HSRP priority 115 on some groups; the task specifies 105 for the preferred active switch and 100 for standby.
- The supplied W2 ISP interface configuration showed bandwidth 15,000 Kbit/s, while W1's ISP interface showed 10,000 Kbit/s. The task states W1's ISP connection must be faster than W2; verify and correct the W2 value against the task specification.
- Complete DHCP lease, SSH allow/deny, Port Security, EtherChannel, HSRP failover/recovery and NAT translation tests.
- Confirm the final W2 NAT ACL correction on the live device.

## Device Configuration Records

The following public configuration records are sanitized. Passwords and password hashes are redacted.

| Device | Configuration record |
|---|---|
| Weil_A1 | [Weil_A1.txt](configurations/Weil_A1.txt) |
| Weil_A2 | [Weil_A2.txt](configurations/Weil_A2.txt) |
| Weil_C1 | [Weil_C1.txt](configurations/Weil_C1.txt) |
| Weil_W1 | [Weil_W1.txt](configurations/routers/Weil_W1.txt) |
| Weil_W2 | [Weil_W2.txt](configurations/routers/Weil_W2.txt) |

[Configuration directory](configurations/README.md)

## Technical Documentation

The detailed documentation is organized by engineering workflow:

| Section | Contents |
|---|---|
| [01 – Network Design](documentation/01-Network-Design/README.md) | Architecture and IP addressing plan |
| [02 – Implementation](documentation/02-Implementation/README.md) | Switching, routing, redundancy, services and security |
| [03 – Testing and Validation](documentation/03-Testing-and-Validation/README.md) | Test plan, observed results, compliance matrix and evidence guide |
| [Evidence library](documentation/evidence/README.md) | Evidence categories and screenshot guidance |

## Repository Contents

```text
Module-C-Weilburg-Network-Configuration/
├── README.md
├── configurations/
│   ├── Weil_A1.txt
│   ├── Weil_A2.txt
│   ├── Weil_C1.txt
│   └── routers/
│       ├── Weil_W1.txt
│       └── Weil_W2.txt
├── documentation/
│   ├── 01-Network-Design/
│   ├── 02-Implementation/
│   ├── 03-Testing-and-Validation/
│   └── evidence/
└── topology/
    ├── Module C – Networking.pka
    ├── Weilburg-Network-Topology.png
    └── README.md
```

## Tools and Technologies

- Cisco Packet Tracer
- Cisco IOS
- VLANs and Layer 3 switching
- LACP EtherChannel
- Rapid PVST+ / 802.1w
- HSRP
- OSPF
- DHCP and DNS
- NAT/PAT
- SSH v2
- Standard and extended ACLs
- Port Security
- IPv4 routing and troubleshooting

---

**Project:** WorldSkills Germany 2025 – IT Network Systems Administration  
**Module:** C – Networking  
**Scenario:** Weilburg Enterprise Network  
**Platform:** Cisco Packet Tracer

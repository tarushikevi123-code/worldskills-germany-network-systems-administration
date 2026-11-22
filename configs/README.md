# Weilburg Network – Device Configurations

This directory contains the Cisco IOS configurations for the five network devices implemented in the WorldSkills Germany 2025 IT Network Systems Administration project.

The configurations document the implementation of a redundant enterprise network infrastructure using Layer 2 and Layer 3 switching, dynamic routing, gateway redundancy, network security, and WAN connectivity.

## Network Devices

| Device | Role | Main Responsibilities |
|---|---|---|
| Weil_A1 | Layer 3 Access Switch 1 | VLANs, Inter-VLAN Routing, STP Root Primary, HSRP |
| Weil_A2 | Layer 3 Access Switch 2 | VLANs, Inter-VLAN Routing, STP Root Secondary, HSRP |
| Weil_C1 | Core Layer 3 Switch | Routing between access switches and WAN routers |
| Weil_W1 | WAN Router 1 | OSPF, Default Routing, NAT/PAT, ACLs |
| Weil_W2 | WAN Router 2 | OSPF, Default Routing, NAT/PAT, ACLs |

## Configuration Files

### 1. Weil_A1.txt – Access Switch 1

The primary Layer 3 access switch.

Implemented configurations:

- VLAN creation and assignment.
- Switch Virtual Interfaces (SVIs).
- Inter-VLAN routing.
- EtherChannel uplink to the core switch.
- Rapid PVST+ (802.1w).
- STP Root Bridge Primary for VLANs 2, 4, 6 and 8.
- HSRP gateway redundancy.
- Active HSRP role for VLAN 2 (HR) and VLAN 4 (Sales).
- DHCP Relay using `ip helper-address`.
- Port Security on access interfaces.
- SSH version 2 and console authentication.
- Standard ACL 1 for restricted remote management access.

### 2. Weil_A2.txt – Access Switch 2

The secondary Layer 3 access switch, providing redundancy.

Implemented configurations:

- VLAN creation and assignment.
- Switch Virtual Interfaces (SVIs).
- Inter-VLAN routing.
- EtherChannel uplink to the core switch.
- Rapid PVST+ (802.1w).
- STP Root Bridge Secondary for VLANs 2, 4, 6 and 8.
- HSRP gateway redundancy.
- Active HSRP role for VLAN 6 (Engineering) and VLAN 8 (WLAN).
- DHCP Relay using `ip helper-address`.
- Port Security on access interfaces.
- SSH version 2 and console authentication.
- Standard ACL 1 for restricted remote management access.

### 3. Weil_C1.txt – Core Switch

The central Layer 3 switching device connecting the access layer to both WAN routers.

Implemented configurations:

- Layer 3 switching.
- EtherChannel connectivity to both access switches.
- Routed interfaces toward Weil_W1 and Weil_W2.
- Server VLAN 130.
- Server network gateway.
- OSPF dynamic routing.
- OSPF Area 0.
- Routing exchange with both access switches and WAN routers.
- Redundant default-route learning from both WAN routers.
- SSH version 2 and console authentication.
- Standard ACL 1 for restricted remote management access.

### 4. Weil_W1.txt – Primary WAN Router

The first WAN gateway connecting the internal enterprise network to external networks.

Implemented configurations:

- WAN and routed interface addressing.
- OSPF Process 1.
- OSPF Router ID 4.4.4.4.
- OSPF Area 0 and Area 2.
- Static default route toward the ISP.
- OSPF default-route advertisement.
- Standard named ACL `vlans` for NAT traffic selection.
- NAT inside and NAT outside configuration.
- Port Address Translation (PAT) using overload.
- Extended named ACL `intranet`.
- Intranet ACL applied inbound on GigabitEthernet0/0/0.
- SSH version 2 and console authentication.
- Standard ACL 1 for restricted remote management access.

### 5. Weil_W2.txt – Secondary WAN Router

The second WAN gateway, providing an additional path toward external networks.

Implemented configurations:

- WAN and routed interface addressing.
- OSPF Process 1.
- OSPF Router ID 5.5.5.5.
- OSPF Area 0 and Area 2.
- Static default route toward the ISP.
- OSPF default-route advertisement.
- Standard named ACL `vlans` for NAT traffic selection.
- NAT inside and NAT outside configuration.
- Port Address Translation (PAT) using overload.
- Extended named ACL `intranet`.
- Intranet ACL applied inbound on GigabitEthernet0/0/0.
- SSH version 2 and console authentication.
- Standard ACL 1 for restricted remote management access.

## Shared Network Security Configuration

The following security mechanisms were implemented across the five network devices:

| Security Feature | Implementation |
|---|---|
| Local Authentication | Local username and password |
| Privileged EXEC Security | Enable secret |
| Console Security | Local authentication |
| Remote Management | SSH version 2 |
| VTY Authentication | Local user database |
| Telnet | Disabled; SSH only |
| VTY Access Control | Standard ACL 1 |
| Domain Name | skill39.org |
| Password Encryption | Enabled |
| MOTD Banner | Unauthorized Access is Strictly Prohibited |

SSH access is restricted to the Server VLAN:

`10.10.130.0/29`

## Network Security and Access Control

### Port Security

Implemented on the two access switches.

- Maximum one MAC address on normal access ports.
- Maximum 50 MAC addresses on the designated WLAN Access Point interface.
- Violation mode: Protect.

### NAT/PAT

A named standard ACL called `vlans` selects the internal source networks permitted for address translation.

PAT uses the ISP-facing Serial0/1/0 interface on both WAN routers.

### Intranet Access Control

A named extended ACL called `intranet` implements the following policy for server `192.168.66.200`:

| Protocol | Destination Port | Policy |
|---|---:|---|
| HTTPS | TCP 443 | Permit |
| HTTP | TCP 80 | Deny |
| Other IP traffic | Any | Permit |

The ACL is applied inbound on GigabitEthernet0/0/0 of both WAN routers.

## Verification

The following network functionality was tested:

- DNS name resolution using the centralized DNS server.
- ICMP connectivity to the simulated external server.
- Intranet HTTPS accessibility.
- Intranet HTTP blocking.
- OSPF neighbor establishment.
- HSRP active and standby gateway roles.
- NAT/PAT translation behavior.

Detailed test evidence and validation status are documented in the `docs/` directory.

## Device Configuration Inventory

| Configuration File | Device | Status |
|---|---|---|
| `Weil_A1.txt` | Weil_A1 | To be included |
| `Weil_A2.txt` | Weil_A2 | To be included |
| `Weil_C1.txt` | Weil_C1 | To be included |
| `Weil_W1.txt` | Weil_W1 | To be included |
| `Weil_W2.txt` | Weil_W2 | To be included |

These configuration files represent the implemented network design and provide a reference for understanding the configuration of each device.

---

**Project:** WorldSkills Germany 2025 – IT Network Systems Administration  
**Platform:** Cisco Packet Tracer  
**Focus:** Enterprise Networking, Routing, Switching, Redundancy and Network Security

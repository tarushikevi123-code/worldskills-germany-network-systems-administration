# Device Configurations – Weilburg Enterprise Network

This directory contains the Cisco IOS configuration exports for all five devices in the Weilburg network implementation.

Each file corresponds to one network device and documents its interface addressing, switching or routing role, network services and security configuration.

## Configuration Files

| File | Device | Role |
|---|---|---|
| `Weil_A1.txt` | Weil_A1 | Layer 3 Access Switch 1 |
| `Weil_A2.txt` | Weil_A2 | Layer 3 Access Switch 2 |
| `Weil_C1.txt` | Weil_C1 | Core Layer 3 Switch |
| `Weil_W1.txt` | Weil_W1 | WAN Router 1 |
| `Weil_W2.txt` | Weil_W2 | WAN Router 2 |

## 1. Weil_A1 – Access Switch 1

The primary Layer 3 access switch for the HR, Sales, Engineering and WLAN VLANs.

The configuration file documents:

- VLAN and access-port configuration.
- Switch Virtual Interfaces and gateway IP addressing.
- Inter-VLAN routing.
- EtherChannel uplink toward Weil_C1.
- Rapid PVST+ and primary STP root placement for VLANs 2, 4, 6 and 8.
- HSRP groups for VLANs 2, 4, 6 and 8, with the intended active role for VLANs 2 and 4.
- DHCP relay on user VLAN interfaces.
- Port Security on access interfaces.
- SSH version 2 and local console authentication.
- Standard ACL 1 on VTY lines for restricted remote management.

## 2. Weil_A2 – Access Switch 2

The secondary Layer 3 access switch providing switching and gateway redundancy.

The configuration file documents:

- VLAN and access-port configuration.
- Switch Virtual Interfaces and gateway IP addressing.
- Inter-VLAN routing.
- EtherChannel uplink toward Weil_C1.
- Rapid PVST+ and secondary STP root placement for VLANs 2, 4, 6 and 8.
- HSRP groups for VLANs 2, 4, 6 and 8, with the intended active role for VLANs 6 and 8.
- DHCP relay on user VLAN interfaces.
- Port Security on access interfaces.
- SSH version 2 and local console authentication.
- Standard ACL 1 on VTY lines for restricted remote management.

## 3. Weil_C1 – Core Layer 3 Switch

The central switching and routing device connecting both access switches to the WAN routers.

The configuration file documents:

- Layer 3 core switching.
- EtherChannel connectivity to Weil_A1 and Weil_A2.
- Routed point-to-point interfaces toward Weil_W1 and Weil_W2.
- Server VLAN 130 and its SVI.
- OSPF Process 1 and router ID.
- OSPF adjacency with access and WAN devices.
- Dynamic routing of internal and external prefixes.
- SSH version 2 and local console authentication.
- Standard ACL 1 on VTY lines for restricted remote management.

## 4. Weil_W1 – WAN Router 1

The first WAN router connecting the Weilburg core network to the ISP and corporate WAN.

The configuration file documents:

- Routed LAN, ISP and corporate WAN interfaces.
- OSPF Process 1, router ID and area assignments.
- Static default route toward the ISP.
- OSPF default-route origination.
- Named standard ACL `vlans` for NAT source selection.
- NAT inside and NAT outside interface roles.
- PAT overload on the ISP-facing interface.
- Named extended ACL `intranet`.
- Inbound application of the Intranet ACL on GigabitEthernet0/0/0.
- SSH version 2, local console authentication and restricted VTY access.

## 5. Weil_W2 – WAN Router 2

The second WAN router providing an additional routed path to external networks.

The configuration file documents:

- Routed LAN, ISP and corporate WAN interfaces.
- OSPF Process 1, router ID and area assignments.
- Static default route toward the ISP.
- OSPF default-route origination.
- Named standard ACL `vlans` for NAT source selection.
- NAT inside and NAT outside interface roles.
- PAT overload on the ISP-facing interface.
- Named extended ACL `intranet`.
- Inbound application of the Intranet ACL on GigabitEthernet0/0/0.
- SSH version 2, local console authentication and restricted VTY access.

## Network-Wide Configuration Areas

The five device exports together document the following implementation areas:

| Technology | Devices |
|---|---|
| VLANs, SVIs and access-port assignment | Weil_A1, Weil_A2, Weil_C1 |
| EtherChannel | Weil_A1, Weil_A2, Weil_C1 |
| Rapid PVST+ | Weil_A1, Weil_A2 |
| HSRP | Weil_A1, Weil_A2 |
| OSPF | Weil_A1, Weil_A2, Weil_C1, Weil_W1, Weil_W2 |
| DHCP relay | Weil_A1, Weil_A2 |
| NAT/PAT | Weil_W1, Weil_W2 |
| SSH and VTY access control | All five devices |
| Intranet extended ACL | Weil_W1, Weil_W2 |
| Port Security | Weil_A1, Weil_A2 |

## Device Configuration Inventory

| Configuration File | Status |
|---|---|
| `Weil_A1.txt` | To be uploaded |
| `Weil_A2.txt` | To be uploaded |
| `Weil_C1.txt` | To be uploaded |
| `Weil_W1.txt` | To be uploaded |
| `Weil_W2.txt` | To be uploaded |


# IPv4 Addressing Plan

This page consolidates the IPv4 networks and service addresses used in the Weilburg topology.

## 1. User and server VLANs

| VLAN | Name | Network | Mask | HSRP virtual IP / SVI |
|---:|---|---|---|---|
| 2 | HR | `10.10.2.0/27` | `255.255.255.224` | HSRP: `10.10.2.30` |
| 4 | SALES | `10.10.4.0/28` | `255.255.255.240` | HSRP: `10.10.4.14` |
| 6 | ENGINEERING | `10.10.6.0/26` | `255.255.255.192` | HSRP: `10.10.6.62` |
| 8 | WLAN | `10.10.8.0/29` | `255.255.255.248` | HSRP: `10.10.8.6` |
| 130 | SERVER | `10.10.130.0/29` | `255.255.255.248` | C1 SVI: `10.10.130.6` |

The user VLAN virtual gateways use the last usable address in each subnet. The server VLAN uses the last usable address on the core SVI.

## 2. Centralized services and application endpoints

| Function | IPv4 address | Notes |
|---|---|---|
| DHCP server | `10.10.130.2` | Central DHCP service for VLANs 2, 4, 6 and 8 |
| DNS server | `10.10.130.3` | DNS service used by the tested client |
| Intranet server | `192.168.66.200` | HTTPS permitted; HTTP denied by the Intranet ACL |
| Simulated external web endpoint | `200.1.1.100` | Returned for `www.google.de` in the observed DNS test |

## 3. Routed site transit links

| Connection | Network | Endpoint addresses |
|---|---|---|
| A1 – C1 | `10.11.1.0/30` | A1: `10.11.1.2`; C1: `10.11.1.1` |
| A2 – C1 | `10.11.2.0/30` | A2: `10.11.2.2`; C1: `10.11.2.1` |
| C1 – W1 | `10.11.3.0/30` | C1: `10.11.3.1`; W1: `10.11.3.2` |
| C1 – W2 | `10.11.4.0/30` | C1: `10.11.4.1`; W2: `10.11.4.2` |

## 4. WAN links

| Connection | Network | Weilburg endpoint | Remote endpoint |
|---|---|---|---|
| W1 – ISP | `195.198.11.0/30` | W1: `195.198.11.1` | ISP: `195.198.11.2` |
| W2 – ISP | `195.198.11.4/30` | W2: `195.198.11.5` | ISP: `195.198.11.6` |
| W1 – Corporate | `172.20.20.0/30` | W1: `172.20.20.1` | Corporate: `172.20.20.2` |
| W2 – Corporate | `172.20.20.4/30` | W2: `172.20.20.5` | Corporate: `172.20.20.6` |

## 5. Loopback management addresses

| Device | Loopback1 | Prefix |
|---|---|---|
| Weil_C1 | `10.10.111.1` | `/32` |
| Weil_A1 | `10.10.111.2` | `/32` |
| Weil_A2 | `10.10.111.3` | `/32` |
| Weil_W1 | `10.10.111.4` | `/32` |
| Weil_W2 | `10.10.111.5` | `/32` |

## 6. OSPF areas and router IDs

| Device | Router ID | Internal site area |
|---|---|---|
| Weil_A1 | `1.1.1.1` | Area 0 |
| Weil_A2 | `2.2.2.2` | Area 0 |
| Weil_C1 | `3.3.3.3` | Area 0 |
| Weil_W1 | `4.4.4.4` | Area 0; Corporate-facing link in Area 2 |
| Weil_W2 | `5.5.5.5` | Area 0; Corporate-facing link in Area 2 |

## 7. NAT source networks

The named standard ACL `vlans` is intended to include these internal networks:

- `10.10.2.0/27` – wildcard `0.0.0.31`
- `10.10.4.0/28` – wildcard `0.0.0.15`
- `10.10.6.0/26` – wildcard `0.0.0.63`
- `10.10.8.0/29` – wildcard `0.0.0.7`
- `10.10.130.0/29` – wildcard `0.0.0.7`

This is the intended address plan. Confirm the current ACL entries on both WAN routers before declaring NAT source selection fully compliant.

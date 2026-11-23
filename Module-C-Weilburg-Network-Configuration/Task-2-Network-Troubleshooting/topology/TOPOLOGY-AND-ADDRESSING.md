# Topology and Addressing Reference

This page records the logical layout shown in **WorldSkills Germany 2025 – Module C, Task 2: Troubleshoot** and the interface/IP mappings confirmed from Packet Tracer screenshots and running-config output.

The official task topology image is not copied into this repository. The Mermaid diagram below is an independent reconstruction for documentation purposes.

## 1. Logical topology

![Uploading image.png…]()


## 2. LAN prefixes from the task diagram

| Segment label | Prefix | Mask | Usable range | Broadcast |
|---|---:|---:|---:|---:|
| LAN Berlin | `192.168.11.0/24` | `255.255.255.0` | `192.168.11.1–192.168.11.254` | `192.168.11.255` |
| LAN Hannover | `192.168.22.0/25` | `255.255.255.128` | `192.168.22.1–192.168.22.126` | `192.168.22.127` |
| LAN Server | `192.168.33.0/28` | `255.255.255.240` | `192.168.33.1–192.168.33.14` | `192.168.33.15` |
| LAN Weilburg | `192.168.44.0/26` | `255.255.255.192` | `192.168.44.1–192.168.44.62` | `192.168.44.63` |
| LAN Weilburg-1 | `192.168.55.0/27` | `255.255.255.224` | `192.168.55.1–192.168.55.30` | `192.168.55.31` |

## 3. Router LAN interfaces observed

| Router | Interface | Address observed after correction | Connected segment |
|---|---|---:|---|
| R-Berlin | Gi0/0/0 | `192.168.11.1/24` | Berlin LAN |
| R-Hannover-1 | Gi0/0/0 | `192.168.22.1/25` | Hannover LAN |
| R-Weil-1 | Gi0/0/0 | `192.168.44.1/26` | Weilburg LAN |
| R-Weil-1 | Gi0/0/1 | `192.168.55.1/27` | Weilburg-1 LAN |
| R-Weil-WAN | Gi0/0/0 | `10.10.1.17/29` | Shared router Ethernet segment |
| R-Berlin | Gi0/0/1 | `10.10.1.18/29` | Shared router Ethernet segment |
| R-Hannover-1 | Gi0/0/1 | `10.10.1.19/29` | Shared router Ethernet segment |

### Server-LAN address consistency check

The topology labels the Server LAN as `192.168.33.0/28`. An intermediate R-Server running-config capture showed `GigabitEthernet0/0/0` with `192.168.33.100 255.255.255.240`. That address belongs to `192.168.33.96/28`, not `192.168.33.0/28`.

A later client test to `192.168.33.100` succeeded, but no final R-Server interface output was captured after the complete repair. Do not treat `192.168.33.100` as the final gateway for the diagram's `192.168.33.0/28` network without checking the saved Packet Tracer file.

## 4. Serial point-to-point link mapping

The correct mapping was established by tracing the physical cable endpoints in Packet Tracer and comparing them with the `/30` prefixes.

| Link | Endpoint A | Endpoint B | Subnet |
|---|---|---|---:|
| R-Weil-WAN ↔ R-Weil-2 | R-Weil-WAN Se0/1/0 – `10.10.0.237` | R-Weil-2 Se0/2/0 – `10.10.0.238` | `10.10.0.236/30` |
| R-Weil-2 ↔ R-Server | R-Weil-2 Se0/1/1 – `10.10.0.241` | R-Server Se0/1/1 – `10.10.0.242` | `10.10.0.240/30` |
| R-Server ↔ R-Weil-1 | R-Server Se0/1/0 – `10.10.0.245` | R-Weil-1 Se0/1/1 – `10.10.0.246` | `10.10.0.244/30` |
| R-Weil-2 ↔ R-Weil-1 | R-Weil-2 Se0/1/0 – `10.10.0.249` | R-Weil-1 Se0/1/0 – `10.10.0.250` | `10.10.0.248/30` |

## 5. Shared router Ethernet segment and DR/BDR

The switch between R-Weil-WAN, R-Berlin and R-Hannover carries the shared subnet `10.10.1.16/29`.

| Router | Interface address | OSPF priority | Observed role |
|---|---:|---:|---|
| R-Weil-WAN | `10.10.1.17/29` | 255 | DR |
| R-Hannover-1 | `10.10.1.19/29` | 1 (default) | BDR |
| R-Berlin | `10.10.1.18/29` | 1 (default) | DROTHER |

These roles were visible in `show ip ospf neighbor` output. The serial links are point-to-point links; they do not elect a DR or BDR.

## 6. Router IDs

| Router | OSPF process | Router ID | Area used on this topology |
|---|---:|---:|---:|
| R-Weil-2 | 10 | `4.4.4.4` | 0 |
| R-Weil-1 | 10 | `5.5.5.5` | 0 |
| R-Server | 10 | `6.6.6.6` | 0 |
| R-Hannover-1 | 10 | `7.7.7.7` | 0 |
| R-Berlin | 10 | `8.8.8.8` | 0 |
| R-Weil-WAN | 10 | `9.9.9.9` | 0 |

## 7. End-host addresses captured during troubleshooting

| Host | Address | Subnet | Notes |
|---|---:|---:|---|
| PC-Hannover | `192.168.22.57` | `/25` | Corrected from an address in the wrong `/25` subnet |
| PC-Weil-1 | `192.168.44.20` | `/26` | Used in captured cross-LAN ping tests |
| PC-Weil-3 | `192.168.55.26` | `/27` | Shown in Packet Tracer IP Configuration and tested |
| PC-Berlin | Not captured | `/24` | Exact host address not inferred |
| PC-Weil-2 | Not captured | `/26` | Exact host address not inferred |
| Server-Berlin | Not captured | `/24` | Exact host address not inferred |
| Server-Hannover | Not captured | `/25` | Exact host address not inferred |
| Server1 | Not captured | See Server-LAN note | Exact host address not inferred |
| LAP-Weil | Not captured | `/27` | Exact host address not inferred |

## 8. Useful subnetting checks

For the two Weilburg LANs:

- `192.168.44.0/26` uses mask `255.255.255.192`; usable hosts are `.1` through `.62`.
- `192.168.55.0/27` uses mask `255.255.255.224`; usable hosts are `.1` through `.30`.

For the shared router segment:

- `10.10.1.16/29` uses mask `255.255.255.248`; usable addresses are `10.10.1.17` through `10.10.1.22`.

For the serial links, `/30` gives two usable host addresses per link. Always map each address to the correct physical serial interface rather than assigning an address based on the interface number alone.

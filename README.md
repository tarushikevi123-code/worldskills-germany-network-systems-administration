# WorldSkills Germany 2025 – IT Network Systems Administration

## Project Overview

This repository documents practical Cisco Packet Tracer work for the WorldSkills Germany 2025 IT Network Systems Administration network tasks.

The projects are organized as independent work folders. Each project has its own README, topology/project files, device configurations and technical documentation.

> Independent educational portfolio work. This repository is not affiliated with or endorsed by WorldSkills Germany or the competition organizers. Official task documents and restricted competition assets are linked rather than redistributed.

## Projects

### Module C – Weilburg Enterprise Network Configuration

**Platform:** Cisco Packet Tracer

Implementation of a redundant enterprise network using:

- VLAN segmentation and Layer 3 switching.
- EtherChannel and Rapid PVST+.
- HSRP gateway redundancy and load sharing.
- OSPF dynamic routing and default-route propagation.
- Centralized DHCP and DNS services.
- NAT/PAT.
- SSH v2 device management and access-control ACLs.
- Port Security.
- Extended ACLs for Intranet HTTP/HTTPS policy.

**Observed verification:** DNS resolution, ICMP reachability to the simulated external server, and Intranet HTTPS/HTTP behavior.

See [Module C – Weilburg Network Configuration](Module-C-Weilburg-Network-Configuration/README.md).

### Module E – Network Troubleshooting

**Platform:** Cisco Packet Tracer

A separate prebuilt multi-router topology with a simulated post-attack configuration failure. The objective is to restore IPv4 ICMP connectivity between the PCs, servers and external server `203.0.113.100`.

The topology-derived addressing reference, discovery workflow and investigation log are maintained separately. Root causes and fixes will be added only after they are confirmed through device outputs and repeatable tests.

See [Module E – Network Troubleshooting](Module-E-Network-Troubleshooting/README.md).

## Technologies and Tools

- Cisco Packet Tracer
- Cisco IOS
- VLANs and Layer 3 Switching
- EtherChannel
- Rapid PVST+
- HSRP
- OSPF
- DHCP and DNS
- NAT/PAT
- SSH
- Standard and Extended ACLs
- Port Security
- IPv4 troubleshooting and ICMP testing

## Repository Structure

```text
.
├── README.md
├── .gitignore
├── Module-C-Weilburg-Network-Configuration/
│   ├── README.md
│   ├── configurations/
│   ├── documentation/
│   └── topology/
└── Module-E-Network-Troubleshooting/
    ├── README.md
    ├── documentation/
    └── topology/
```

## Project Status

| Project | Status |
|---|---|
| Module C – Weilburg Network Configuration | Implemented; selected functional tests completed; final compliance audit and evidence collection pending |
| Module E – Network Troubleshooting | Investigation documentation prepared; practical troubleshooting work in progress |

## Official References

- [WorldSkills Germany 2025 – IT Network Systems Administration](https://itnsa.de/meisterschaften/weilburg-2025/)
- [Module C – Task Document](https://itnsa.de/static/WSG25_TP39_MC.pdf)
- [Module E – Task Document](https://itnsa.de/static/WSG25_TP39_ME.pdf)

Official starter files, assessment documents and other third-party competition material are not redistributed in this repository.

---

**Project Area:** Network Engineering / Routing & Switching / Network Troubleshooting  
**Platform:** Cisco Packet Tracer

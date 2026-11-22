# WorldSkills Germany 2025 — IT Network Systems Administration

A hands-on Cisco Packet Tracer portfolio project documenting an enterprise network implementation and a separate multi-router troubleshooting exercise based on the WorldSkills Germany 2025 IT Network Systems Administration materials.

> **Independent educational work.** This repository is not affiliated with or endorsed by WorldSkills Germany or the competition organizers. Official competition PDFs, starter topologies, assessment material, and other third-party assets are linked rather than redistributed.

## Project overview

| Item | Details |
|---|---|
| Platform | Cisco Packet Tracer |
| Main configuration scenario | Weilburg campus network with access, core, dual WAN, client, and server segments |
| Technologies | VLANs, Layer 3 switching, EtherChannel, Rapid PVST+, HSRP, OSPF, DHCP relay, DNS, NAT/PAT, SSH, and ACLs |
| Separate troubleshooting scenario | Restore IPv4 reachability across multiple routed LANs and to an external test server |
| Method | Discover → form a hypothesis → change one item → verify → document |

## Workstream 1 — Weilburg network implementation

The implementation work covers:

- VLAN segmentation for HR, Sales, Engineering, WLAN, and server services.
- Layer 3 switching and inter-VLAN gateway redundancy with HSRP.
- Aggregated uplinks using EtherChannel.
- Loop prevention and root-bridge placement using Rapid PVST+.
- OSPF routing across the Weilburg access/core and dual-WAN design.
- DHCP relay to centralized DHCP and DNS services.
- NAT/PAT toward the simulated ISP.
- SSH v2 device management, local authentication, and source-restricted VTY access.
- Port security on access-switch ports.
- Extended ACL policy for the Intranet server: HTTPS allowed, HTTP denied, other IP traffic permitted.

### Devices and roles

| Device | Role |
|---|---|
| `Weil_A1` | Layer 3 access/distribution switch; preferred HSRP active for VLANs 2 and 4 |
| `Weil_A2` | Layer 3 access/distribution switch; preferred HSRP active for VLANs 6 and 8 |
| `Weil_C1` | Core Layer 3 switch |
| `Weil_W1` | WAN edge router 1 |
| `Weil_W2` | WAN edge router 2 |

### Addressing reference

| Segment | Network | Service / gateway notes |
|---|---|---|
| VLAN 2 — HR | `10.10.2.0/27` | HSRP virtual gateway `10.10.2.30` |
| VLAN 4 — Sales | `10.10.4.0/28` | HSRP virtual gateway `10.10.4.14` |
| VLAN 6 — Engineering | `10.10.6.0/26` | HSRP virtual gateway `10.10.6.62` |
| VLAN 8 — WLAN | `10.10.8.0/29` | HSRP virtual gateway `10.10.8.6` |
| VLAN 130 — Servers | `10.10.130.0/29` | DHCP `10.10.130.2`; DNS `10.10.130.3`; core SVI `10.10.130.6` |
| A1 ↔ Core | `10.11.1.0/30` | Weil_A1 to Weil_C1 |
| A2 ↔ Core | `10.11.2.0/30` | Weil_A2 to Weil_C1 |
| Core ↔ WAN1 | `10.11.3.0/30` | Weil_C1 to Weil_W1 |
| Core ↔ WAN2 | `10.11.4.0/30` | Weil_C1 to Weil_W2 |

See [Network overview](docs/NETWORK-OVERVIEW.md) for the logical diagram, services, and validation checklist.

## Workstream 2 — Separate network troubleshooting exercise

A separate supplied Packet Tracer topology starts from a damaged multi-router network. The task statement indicates that the IP ranges shown in the topology remain valid while other configuration information must be rediscovered. The objective is to restore IPv4 ICMP reachability among the PCs and servers and to the external server `203.0.113.100`.

The discovery checklist and topology-derived prefixes are recorded in [Troubleshooting log](docs/TROUBLESHOOTING-LOG.md). No root cause or repair is claimed until it has been observed and verified in the working Packet Tracer file.

## Verification performed so far

These tests were reported from a Packet Tracer client:

| Test | Observed result | Demonstrates |
|---|---|---|
| `nslookup www.google.de` | DNS server `10.10.130.3` returned `200.1.1.100` | DNS resolution works for the tested record |
| `ping www.google.de` | 4 replies, 0% loss | DNS resolution and ICMP reachability to the simulated external server |
| `ping 200.1.1.100` | 4 replies, 0% loss | Direct ICMP reachability to the simulated external server |
| Intranet HTTPS test | Browser access to `https://192.168.66.200` succeeded | HTTPS permitted on the tested path |
| Intranet HTTP test | Browser access to `http://192.168.66.200` was blocked | HTTP denied on the tested path |

These are observed tests, not a claim that every rubric item has passed. The final requirement-by-requirement audit remains pending. See [Test results](docs/TEST-RESULTS.md) and the [Validation matrix](docs/VALIDATION-MATRIX.md).

## Repository structure

```text
.
├── README.md
├── .gitignore
├── configs/                      # Sanitized command notes and device exports
├── docs/
│   ├── EVIDENCE-STANDARD.md
│   ├── NETWORK-OVERVIEW.md
│   ├── TEST-RESULTS.md
│   ├── TROUBLESHOOTING-LOG.md
│   └── VALIDATION-MATRIX.md
├── evidence/
│   ├── module-c-configuration/   # Sanitized screenshots and captions
│   └── troubleshooting/          # Before/after evidence
└── packet-tracer/                # Only files approved for redistribution
```

## Official references

- [WorldSkills Germany 2025 — ITNSA competition page](https://itnsa.de/meisterschaften/weilburg-2025/)
- [Module C — official task document](https://itnsa.de/static/WSG25_TP39_MC.pdf)
- [Module C — official configuration starter](https://itnsa.de/static/WSG25_TP39_MC_Configuration.pka)
- [Module C — official troubleshooting starter](https://itnsa.de/static/WSG25_TP39_MC_Troublesshoot.pka)
- [Module E — official task document](https://itnsa.de/static/WSG25_TP39_ME.pdf)
- [Module E — assessment guide](https://itnsa.de/static/WSG25_TP39_ME_Assessment.pdf)

Use the official competition page for authoritative versions of source material.

## Security and redistribution

- Never commit clear-text passwords, password hashes, enable secrets, tokens, private keys, or unredacted device backups.
- Sanitize configuration exports before publishing. See [configuration guidance](configs/README.md).
- Packet Tracer project files are excluded by default. Add a `.pka` or `.pkt` file only after confirming redistribution is permitted and reviewing it for embedded credentials and sensitive content.
- Do not re-upload competition PDFs, starter files, assessment guides, or other third-party material without explicit permission.
- No license is included yet. A license for original notes or configurations cannot grant rights to third-party competition material.

## Current status

- [x] Weilburg configuration work completed to a working state.
- [x] DNS lookup for the simulated external hostname verified.
- [x] ICMP reachability to the simulated external server verified (4/4 replies in the recorded test).
- [x] Intranet HTTPS/HTTP ACL behavior tested.
- [ ] Complete final rubric audit (including HSRP priority/tracking and AP-port assignment).
- [ ] Add sanitized device configuration exports.
- [ ] Add sanitized screenshots and captions.
- [ ] Complete the separate troubleshooting exercise and document confirmed root causes and fixes.
- [ ] Add a Packet Tracer working file only if redistribution is permitted.

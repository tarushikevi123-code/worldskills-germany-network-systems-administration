# Module E – Network Troubleshooting

## 1. Project Overview

This project documents the investigation and recovery of a prebuilt multi-router network in Cisco Packet Tracer. The task describes a simulated hacker attack after which network configurations were changed or deleted.

The topology itself is supplied. The IP address ranges shown in the topology diagram remain valid, while the device configurations and other network documentation must be investigated.

**Platform:** Cisco Packet Tracer

## 2. Objective

Restore IPv4 ICMP connectivity as quickly as possible so that:

- All PCs can communicate with each other.
- All servers can communicate with each other.
- PCs and servers can reach the external server at `203.0.113.100`.

The supplied task states that all routers and switches share a task-specific access password. Credentials are intentionally not reproduced in this public repository.

## 3. Supplied Network Topology

The prebuilt topology includes the following routers and LAN segments:

- R-Weil-WAN – Weilburg WAN edge.
- R-Weil-2 and R-Weil-1 – Weilburg internal routing devices.
- R-Server – server network router.
- R-Berlin – Berlin LAN router.
- R-Hannover – Hannover LAN router.
- PCs and servers distributed across the LANs.
- External server – `203.0.113.100`.

The topology is provided by the task; the objective is to troubleshoot and restore it, not to recreate the devices and cabling from scratch.

## 4. Addressing Reference

The following prefixes are transcribed from the supplied topology diagram and should be confirmed against the actual Packet Tracer file before configuring interfaces.

| Segment / Link | Prefix |
|---|---|
| Berlin LAN | `192.168.11.0/24` |
| Hannover LAN | `192.168.22.0/25` |
| Server LAN | `192.168.33.0/28` |
| Weilburg LAN | `192.168.4.0/26` |
| Weilburg LAN 1 | `192.168.5.0/27` |
| R-Weil-WAN ↔ R-Weil-2 | `10.10.0.236/30` |
| R-Weil-2 ↔ R-Server | `10.10.0.240/30` |
| R-Server ↔ R-Weil-1 | `10.10.0.244/30` |
| R-Weil-2 ↔ R-Weil-1 | `10.10.0.248/30` |
| R-Weil-WAN ↔ upstream segment | `10.10.1.16/29` |
| External server | `203.0.113.100` |

## 5. Troubleshooting Methodology

The investigation follows a structured approach:

1. Establish baseline connectivity from representative PCs and servers.
2. Record host addressing, subnet masks and default gateways.
3. Discover router interface status and IPv4 addressing.
4. Inspect routing tables and routing protocol configuration.
5. Verify OSPF neighbors and advertised networks where relevant.
6. Test connectivity hop by hop: local gateway, adjacent router, remote LAN and external server.
7. Change one item at a time and repeat the affected test.
8. Record the confirmed cause, corrective action and post-fix verification.

## 6. Verification Commands

### Router discovery

```cisco
show ip interface brief
show ip route
show ip protocols
show running-config
```

### OSPF inspection

```cisco
show ip ospf neighbor
show ip ospf interface brief
show ip route ospf
```

### End-to-end testing

```text
ipconfig
ping <default-gateway>
ping <remote-host>
ping 203.0.113.100
tracert 203.0.113.100
```

## 7. Project Files

- Cisco Packet Tracer troubleshooting topology (to be added if permitted for redistribution).
- Device configuration notes.
- Investigation log.
- Before-and-after screenshots.
- Connectivity verification evidence.

## 8. Project Status

**Topology:** Supplied prebuilt topology; no recreation required.

**Investigation:** Documentation and addressing reference prepared.

**Root causes and fixes:** Not yet recorded. Findings will be documented only after they are confirmed through Packet Tracer command output and repeatable tests.

---

**Project Area:** Network Engineering / Routing / Troubleshooting  
**Platform:** Cisco Packet Tracer

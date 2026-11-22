# Troubleshooting log — separate Packet Tracer scenario

## Objective

Restore IPv4 ICMP reachability between all PCs and servers and the external server `203.0.113.100` after a simulated attack changed or removed device configurations.

## Constraints and starting conditions

- The topology diagram provides the IP address ranges that remain valid.
- Other network documentation has been removed, so interface addresses and device configuration must be discovered from the running topology.
- The task provides a common device access password. Do not publish it or any credentials in this repository.
- Work from the supplied Packet Tracer file; do not rebuild the topology unless the starter file is genuinely incomplete.

## Addressing transcribed from the topology diagram

| Segment / link | Prefix shown |
|---|---:|
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

These prefixes are transcribed from the supplied topology image. Verify interface masks and endpoint addresses from the actual Packet Tracer file before configuring devices.

## Investigation method

1. Establish a baseline from representative end hosts.
2. Record host IP address, subnet mask, default gateway, and DNS settings.
3. Discover router interfaces using `show ip interface brief`.
4. Inspect `show ip route`, `show ip protocols`, and OSPF neighbor state where applicable.
5. Test connectivity one hop at a time: local gateway, adjacent router, remote LAN, then external server.
6. Make one change at a time and repeat the affected test.
7. Record root cause, evidence, fix, and the final verification result.

## Investigation log

| Step | Device / host | Command or test | Observation | Hypothesis / action | Result |
|---|---|---|---|---|---|
| 1 | Not yet recorded | `ping 203.0.113.100` from an end host | Baseline not yet recorded | Establish initial reachability state | Pending |

## Discovery checklist

- [ ] Record `show ip interface brief` on each router.
- [ ] Record `show ip route` and `show ip protocols` on each router.
- [ ] Check OSPF neighbors, areas, and advertised networks.
- [ ] Check interface status, addressing, encapsulation, and line protocol.
- [ ] Check each host's IP address, mask, gateway, and DNS settings.
- [ ] Test each LAN gateway, adjacent router hop, remote LAN, and external server sequentially.
- [ ] Save a checkpoint before making changes.
- [ ] Record each confirmed root cause, exact repair, and post-fix verification.

## Findings and fixes

No root cause or repair is recorded yet. Add findings only after they are confirmed by command output or repeatable connectivity tests.

# Routing and Redundancy Implementation

## 1. Layer 3 links

The core switch uses routed links towards the WAN routers. The access switches use L3 EtherChannels towards the core.

| Link | Network | Area |
|---|---|---:|
| A1 – C1 | `10.11.1.0/30` | 0 |
| A2 – C1 | `10.11.2.0/30` | 0 |
| C1 – W1 | `10.11.3.0/30` | 0 |
| C1 – W2 | `10.11.4.0/30` | 0 |
| W1 – Corporate | `172.20.20.0/30` | 2 |
| W2 – Corporate | `172.20.20.4/30` | 2 |
| W1 – ISP | `195.198.11.0/30` | 0 in captured W1 process |
| W2 – ISP | `195.198.11.4/30` | 0 in captured W2 process |

The WAN routers operate as OSPF Area Border Routers between the Weilburg site (Area 0) and corporate network (Area 2).

## 2. OSPF process

OSPF Process 1 is used. Router IDs:

| Device | Router ID |
|---|---|
| Weil_A1 | `1.1.1.1` |
| Weil_A2 | `2.2.2.2` |
| Weil_C1 | `3.3.3.3` |
| Weil_W1 | `4.4.4.4` |
| Weil_W2 | `5.5.5.5` |

The task specifies exact interface IP network statements with wildcard `0.0.0.0`. User-facing access VLANs 2, 4, 6 and 8 should not send OSPF updates.

## 3. WAN bandwidth and default routing

The W1 ISP connection is intended to be faster than W2. The captured W1 ISP interface shows bandwidth 10,000 Kbit/s, and W1's corporate interface shows 5,000 Kbit/s. The W2 captured ISP interface shows 15,000 Kbit/s, while its corporate interface has no bandwidth command in the supplied running configuration. The ISP-side bandwidth relationship therefore needs a final check against the task's Quick Specifications and current W2 requirements; do not infer a required W2 value from W1.

Both WAN routers have a static default route using the ISP next-hop address and use `default-information originate` under OSPF.

Examples from the supplied running configurations:

- W1: `ip route 0.0.0.0 0.0.0.0 195.198.11.2`
- W2: `ip route 0.0.0.0 0.0.0.0 195.198.11.6`

## 4. Observed adjacency state

Operational output shared during troubleshooting showed:

- W1 – C1 neighbor `3.3.3.3`: FULL.
- W1 – Corporate neighbor `6.6.6.6`: EXSTART in the latest detailed neighbor output shared at that point.
- W1's Corporate serial interface was reported up/up, MTU 1500 bytes, and a ping to `172.20.20.2` succeeded 5/5.
- W2 output later showed C1 neighbor `3.3.3.3` FULL and Corporate neighbor `6.6.6.6` FULL at `172.20.20.6`.

These are time-specific observations, not proof of the current state. Recheck W1 before recording full OSPF convergence:

```cisco
show ip ospf neighbor
show ip ospf interface brief
show ip route
show ip route 0.0.0.0
```

The attempted `ip ospf mtu-ignore` command was not available in the Packet Tracer IOS image used for W1. Do not document it as configured.

## 5. Failover verification

To test routing resilience, capture the routing table and neighbor state before and after a controlled link or device failure. Record the exact interface disabled, expected alternate path, packet loss/convergence behavior, and restoration result. Only mark the test complete when the operational evidence is collected.

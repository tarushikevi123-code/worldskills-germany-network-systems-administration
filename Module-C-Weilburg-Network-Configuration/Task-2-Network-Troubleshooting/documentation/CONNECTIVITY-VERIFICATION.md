# Connectivity Verification Results

This page summarizes the IPv4 ICMP tests captured during the final validation stage. Each listed test sent four echo requests and received four replies unless noted otherwise.

## 1. External server

The external test target was `203.0.113.100`.

## 2. Captured endpoint tests

### PC-Berlin

| Destination | Description | Sent | Received | Loss |
|---|---|---:|---:|---:|
| `203.0.113.100` | External server | 4 | 4 | 0% |
| `192.168.11.1` | Berlin router LAN interface | 4 | 4 | 0% |
| `192.168.22.1` | Hannover router LAN interface | 4 | 4 | 0% |
| `192.168.33.100` | Server-LAN-side address tested during the session | 4 | 4 | 0% |
| `192.168.44.1` | Weilburg router LAN interface | 4 | 4 | 0% |
| `192.168.55.1` | Weilburg-1 router LAN interface | 4 | 4 | 0% |

**Result:** 24/24 replies across six destinations, 0% loss.

### PC-Hannover (`192.168.22.57/25`)

| Destination | Description | Sent | Received | Loss |
|---|---|---:|---:|---:|
| `203.0.113.100` | External server | 4 | 4 | 0% |
| `192.168.11.1` | Berlin router LAN interface | 4 | 4 | 0% |
| `192.168.22.1` | Hannover default gateway | 4 | 4 | 0% |
| `192.168.33.100` | Server-LAN-side address tested during the session | 4 | 4 | 0% |
| `192.168.44.1` | Weilburg router LAN interface | 4 | 4 | 0% |
| `192.168.55.1` | Weilburg-1 router LAN interface | 4 | 4 | 0% |
| `192.168.44.20` | PC-Weil-1 | 4 | 4 | 0% |
| `192.168.55.26` | PC-Weil-3 | 4 | 4 | 0% |

**Result:** 32/32 replies across eight destinations, 0% loss.

### PC-Weil-1 (`192.168.44.20/26`)

| Destination | Description | Sent | Received | Loss |
|---|---|---:|---:|---:|
| `192.168.44.1` | Weilburg default gateway | 4 | 4 | 0% |
| `192.168.22.57` | PC-Hannover | 4 | 4 | 0% |
| `192.168.11.1` | Berlin router LAN interface | 4 | 4 | 0% |
| `192.168.22.1` | Hannover router LAN interface | 4 | 4 | 0% |
| `192.168.33.100` | Server-LAN-side address tested during the session | 4 | 4 | 0% |
| `203.0.113.100` | External server | 4 | 4 | 0% |

**Result:** 24/24 replies across six destinations, 0% loss.

### PC-Weil-3 (`192.168.55.26/27`)

| Destination | Description | Sent | Received | Loss |
|---|---|---:|---:|---:|
| `192.168.55.1` | Weilburg-1 default gateway | 4 | 4 | 0% |
| `203.0.113.100` | External server | 4 | 4 | 0% |

**Result:** 8/8 replies across two destinations, 0% loss.

### LAP-Weil

| Destination | Description | Sent | Received | Loss |
|---|---|---:|---:|---:|
| `203.0.113.100` | External server | 4 | 4 | 0% |

**Result:** 4/4 replies, 0% loss.

## 3. Consolidated captured results

| Source endpoint | Destinations tested | Requests sent | Replies received | Packet loss |
|---|---:|---:|---:|---:|
| PC-Berlin | 6 | 24 | 24 | 0% |
| PC-Hannover | 8 | 32 | 32 | 0% |
| PC-Weil-1 | 6 | 24 | 24 | 0% |
| PC-Weil-3 | 2 | 8 | 8 | 0% |
| LAP-Weil | 1 | 4 | 4 | 0% |
| **Total captured** | **23** | **92** | **92** | **0%** |

This table aggregates the explicit ping outputs captured in the working session. It should not be interpreted as a complete all-to-all matrix for every PC and server.

## 4. Operator-reported final outcome

After the captured tests, the operator stated that all devices could communicate with each other and reach the external server. This is recorded as the final lab outcome reported by the operator.

The following detailed per-host evidence was not captured in the conversation and should be added from Packet Tracer screenshots or the saved lab if available:

- PC-Weil-2 individual IP configuration and ping results.
- Server-Berlin ping results to other servers/PCs and to the external server.
- Server-Hannover ping results to other servers/PCs and to the external server.
- Server1 ping results to other servers/PCs and to the external server.
- A complete all-to-all host reachability matrix.

## 5. Repeatable test procedure

On each PC/server in Packet Tracer:

```text
ipconfig
ping <local-default-gateway>
ping <remote-PC-address>
ping <remote-server-address>
ping 203.0.113.100
```

Record the source device, destination address, sent/received/lost counts and any first-packet timeout. First-packet timeout can occur in Packet Tracer while ARP resolution is taking place; repeat the test and record the final result rather than treating a single initial timeout as a persistent failure.

## 6. Evidence checklist

- [x] PC-Berlin to internal gateway/interface addresses and external server.
- [x] PC-Hannover to internal gateway/interface addresses, two Weilburg PCs and external server.
- [x] PC-Weil-1 to gateway, PC-Hannover, internal router interfaces and external server.
- [x] PC-Weil-3 to its gateway and external server.
- [x] LAP-Weil to external server.
- [ ] Add individual server-origin ping evidence if captured.
- [ ] Add PC-Weil-2 host-level ping evidence if captured.
- [ ] Add full all-to-all host test matrix if a formal evidence pack is required.
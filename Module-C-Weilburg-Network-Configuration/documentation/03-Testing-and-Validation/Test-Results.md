# Test Results – Weilburg Network

This file records actual observations shared from the Packet Tracer environment. It is not a complete requirements sign-off.

## 1. DNS resolution

**Test:** Resolve `www.google.de` from a client.

**Command:**

```text
nslookup www.google.de
```

**Observed output:**

```text
Server:  [10.10.130.3]
Address: 10.10.130.3
Name:   www.google.de
Address: 200.1.1.100
```

**Result:** DNS server `10.10.130.3` returned the simulated endpoint `200.1.1.100`.

## 2. Simulated external endpoint reachability

**Commands:**

```text
ping www.google.de
ping 200.1.1.100
```

**Observed result:** Both tests returned 4 replies from 4 packets sent (0% packet loss).

**Scope:** This confirms the tested client's DNS resolution and ICMP reachability to the simulated endpoint. It does not demonstrate reachability to arbitrary public Internet destinations.

## 3. Intranet access policy

**Target:** `192.168.66.200`

**Configured ACL policy:**

- Permit TCP/443 to the Intranet server.
- Deny TCP/80 to the Intranet server.
- Permit all other IP traffic.

ACL placement was shown inbound on GigabitEthernet0/0/0 of both WAN routers:

- W1 – `10.11.3.2/30`
- W2 – `10.11.4.2/30`

**Client browser observations:**

| URL | Observed result |
|---|---|
| `https://192.168.66.200` | Succeeded |
| `http://192.168.66.200` | Blocked |

## 4. OSPF observations during troubleshooting

The following device outputs were shared during a separate troubleshooting session:

| Device | Neighbor | Observed state | Context |
|---|---|---|---|
| W1 | C1 – router ID `3.3.3.3` | FULL | Neighbor reported on the C1-facing GigabitEthernet link |
| W1 | Corporate – router ID `6.6.6.6` | EXSTART | Latest detailed neighbor output at that point; W1 serial link was up/up and ping to `172.20.20.2` succeeded 5/5 |
| W2 | C1 – router ID `3.3.3.3` | FULL | Output shared from W2 |
| W2 | Corporate – router ID `6.6.6.6` | FULL | Neighbor address `172.20.20.6` |

These are time-specific observations. Recheck the live neighbor table before claiming end-to-end OSPF convergence. Later W1 output has not been supplied in this record.

## 5. Remaining tests

The following items have not yet been evidenced in the current documentation:

- DHCP leases and DNS options in each user VLAN.
- VLAN-to-VLAN reachability for representative endpoints.
- EtherChannel status and member-failure behavior.
- STP root placement and access-port safeguards.
- HSRP priority, tracking, failover and recovery.
- Full OSPF convergence and default-route propagation across the complete topology.
- NAT/PAT translations and return traffic.
- SSH permitted and denied source tests.
- Port Security limits, Protect behavior and AP-port compliance.

## 6. Evidence to capture

| Suggested filename | Evidence |
|---|---|
| `dns-nslookup.png` | DNS server and returned address |
| `external-ping.png` | Successful ICMP test to the simulated endpoint |
| `intranet-https-allowed.png` | HTTPS browser success |
| `intranet-http-blocked.png` | HTTP browser denial |
| `ospf-neighbors-w1.png` | Current W1 neighbor table |
| `ospf-neighbors-w2.png` | Current W2 neighbor table |

Place screenshots in the relevant subfolder under the repository's Module C `evidence/` directory.

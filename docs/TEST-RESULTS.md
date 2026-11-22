# Test results — current observed evidence

This page records tests actually reported from the Packet Tracer client. It is not a full rubric sign-off.

## DNS and simulated external-server reachability

**Client command:**

```text
nslookup www.google.de
```

**Observed response:**

```text
Server:  [10.10.130.3]
Address: 10.10.130.3
Name:    www.google.de
Address: 200.1.1.100
```

**Client command:**

```text
ping www.google.de
```

**Observed result:** 4 packets sent, 4 received, 0% loss.

**Client command:**

```text
ping 200.1.1.100
```

**Observed result:** 4 packets sent, 4 received, 0% loss.

**Interpretation:** The tested client received a DNS answer from `10.10.130.3` and successfully exchanged ICMP packets with the simulated external server `200.1.1.100`. This does not imply that arbitrary public Internet destinations such as `8.8.8.8` are reachable in the Packet Tracer simulation.

## Intranet ACL behavior

**Target server:** `192.168.66.200`

**ACL intent:** Permit TCP/443 to the server, deny TCP/80 to the server, and permit other IP traffic.

**Placement observed:** ACL `intranet` applied inbound on GigabitEthernet0/0/0 on Weil_W1 (`10.11.3.2/30`) and Weil_W2 (`10.11.4.2/30`). Both WAN-router outputs showed the ACL as the inbound access list on that interface.

**Client browser tests reported:**

- `https://192.168.66.200` — succeeded.
- `http://192.168.66.200` — blocked.

**Interpretation:** The reported browser behavior matches the intended HTTP/HTTPS policy for the tested path.

## Evidence to attach

Add sanitized screenshots under `../evidence/module-c-configuration/` and refer to them here with captions. Recommended evidence:

| Suggested filename | Evidence |
|---|---|
| `dns-nslookup.png` | `nslookup www.google.de` response showing DNS server and returned address |
| `external-hostname-ping.png` | Successful 4/4 ping to `www.google.de` |
| `external-ip-ping.png` | Successful 4/4 ping to `200.1.1.100` |
| `intranet-https-allowed.png` | Successful HTTPS browser access |
| `intranet-http-blocked.png` | HTTP request blocked |
| `wan1-intranet-acl.png` | ACL and inbound interface verification on Weil_W1 |
| `wan2-intranet-acl.png` | ACL and inbound interface verification on Weil_W2 |

## Pending validation

- Full VLAN-to-VLAN reachability tests from representative hosts.
- HSRP failover and recovery behavior, including priority and tracking.
- EtherChannel member-failure behavior.
- OSPF failover and route convergence.
- DHCP lease verification in each client VLAN.
- SSH connection tests from an authorized Server VLAN source and a denied non-Server VLAN source.
- Port-security violation behavior and final AP-port compliance.
- Full requirement-by-requirement review against the official task document.

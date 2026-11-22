# Test Results – Weilburg Network

This page records test results reported from the Packet Tracer client. It is not a complete rubric sign-off.

## DNS resolution and simulated external-server reachability

Command:

```text
nslookup www.google.de
```

Observed result:

```text
Server:  [10.10.130.3]
Address: 10.10.130.3
Name:   www.google.de
Address: 200.1.1.100
```

The configured DNS server `10.10.130.3` returned the simulated external-server address `200.1.1.100`.

Command:

```text
ping www.google.de
ping 200.1.1.100
```

Observed result for both tests: 4 packets sent, 4 received, 0% loss.

This demonstrates DNS resolution and ICMP reachability to the simulated external server from the tested client. It does not demonstrate reachability to arbitrary public Internet destinations such as `8.8.8.8`.

## Intranet ACL behavior

Target server: `192.168.66.200`

ACL policy:
- Permit TCP/443 to the Intranet server.
- Deny TCP/80 to the Intranet server.
- Permit all other IP traffic.

ACL placement was verified inbound on GigabitEthernet0/0/0 of both WAN routers:
- Weil_W1 – `10.11.3.2/30`
- Weil_W2 – `10.11.4.2/30`

Client browser tests reported:
- `https://192.168.66.200` – succeeded.
- `http://192.168.66.200` – blocked.

## Screenshots to include

Store sanitized screenshots under `documentation/evidence/` and use captions explaining what each one demonstrates.

| Suggested filename | Evidence |
|---|---|
| `dns-nslookup.png` | DNS server and returned address from `nslookup www.google.de` |
| `external-hostname-ping.png` | Successful 4/4 ping to `www.google.de` |
| `external-ip-ping.png` | Successful 4/4 ping to `200.1.1.100` |
| `intranet-https-allowed.png` | Successful HTTPS browser access |
| `intranet-http-blocked.png` | HTTP request blocked |
| `wan1-intranet-acl.png` | ACL and inbound interface verification on Weil_W1 |
| `wan2-intranet-acl.png` | ACL and inbound interface verification on Weil_W2 |

## Pending verification

- VLAN-to-VLAN reachability from representative hosts.
- HSRP failover and recovery, including priority and tracking.
- EtherChannel member-failure behavior.
- OSPF failover and route convergence.
- DHCP lease verification in every client VLAN.
- SSH connection tests from an authorized Server VLAN source and a denied non-Server VLAN source.
- Port Security violation behavior and final AP interface compliance.
- Full requirement-by-requirement review against the official task document.

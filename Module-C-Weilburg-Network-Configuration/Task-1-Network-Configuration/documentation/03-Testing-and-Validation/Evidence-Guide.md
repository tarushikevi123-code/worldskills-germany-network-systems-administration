# Evidence Guide

Use focused evidence that allows another engineer to understand what was tested and what the result proves.

## Capture checklist

- Keep the Packet Tracer device name and command visible in CLI screenshots.
- Include the output lines that demonstrate the result; avoid unnecessary screen content.
- Do not publish passwords, password hashes, keys, tokens or unrelated personal information.
- Prefer focused `show` command output over entire running configurations.
- For client tests, show the client identity, its VLAN/addressing context, destination and result.
- For ACL tests, show both the policy and the functional permit/deny result.
- For failover tests, capture the state before failure, during failure and after recovery.
- Use descriptive filenames and reference them from the corresponding test result.

## Evidence folders

| Folder | Suitable evidence |
|---|---|
| `evidence/topology/` | Overall logical or Packet Tracer topology |
| `evidence/switching/` | VLANs, trunks, EtherChannel, STP and Port Security |
| `evidence/routing/` | OSPF neighbors, routes, HSRP roles and failover |
| `evidence/services/` | DHCP leases, DNS resolution and NAT translations |
| `evidence/security/` | SSH access tests and Intranet ACL behavior |

## Naming pattern

Use:

```text
<topic>-<device-or-client>-<test>.png
```

Examples:

- `etherchannel-core-switch-summary.png`
- `ospf-neighbors-w1.png`
- `hsrp-vlan2-before-failover.png`
- `dhcp-lease-vlan4-client.png`
- `intranet-http-blocked.png`

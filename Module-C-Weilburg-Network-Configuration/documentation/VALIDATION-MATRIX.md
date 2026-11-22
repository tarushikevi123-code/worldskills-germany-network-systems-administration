# Validation Matrix

Status reflects evidence recorded so far. `Pending` means not yet evidenced, not necessarily broken.

| Area | Verification | Status | Evidence / note |
|---|---|---|---|
| DNS | `nslookup www.google.de` from a client | Verified | DNS `10.10.130.3` returned `200.1.1.100` |
| External reachability | `ping www.google.de` and `ping 200.1.1.100` | Verified | Both returned 4/4 replies |
| Intranet HTTPS | Browser request to `https://192.168.66.200` | Verified | Reported successful |
| Intranet HTTP | Browser request to `http://192.168.66.200` | Verified | Reported blocked |
| VLAN-to-VLAN connectivity | Test representative clients in each VLAN | Pending | Record source, destination and result |
| DHCP | Confirm lease and DNS option in each client VLAN | Pending | DHCP `10.10.130.2`; DNS `10.10.130.3` |
| EtherChannel | Verify bundle state and member failure behavior | Pending | Capture `show etherchannel summary` |
| STP | Verify Rapid PVST+ and root placement | Pending | Capture `show spanning-tree` for required VLANs |
| HSRP | Verify active/standby roles, priority, preemption, tracking and failover | Pending | Final priority/tracking audit remains open |
| OSPF | Verify neighbors, areas, passive interfaces and default routes | Pending | Capture `show ip ospf neighbor` and route table |
| NAT/PAT | Verify live translations and return traffic | Pending | Capture `show ip nat translations` during a test |
| SSH | Verify SSH v2 and authorized/unauthorized source behavior | Pending | ACL 1 and VTY settings require end-to-end tests |
| Port Security | Verify MAC limits, Protect action and AP interface | Pending | AP interface assignment requires final compliance review |

## Recording a test

For each test, record source device/VLAN, destination, exact command or protocol, expected result, observed result, screenshot filename and any limitations.

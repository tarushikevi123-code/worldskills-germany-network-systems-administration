# Validation matrix

Status reflects evidence recorded in this repository at the time of writing. `Pending` means not yet evidenced; it does not necessarily mean the configuration is broken.

| Area | Verification | Status | Evidence / note |
|---|---|---|---|
| DNS | `nslookup www.google.de` from a client | Verified | Resolved by `10.10.130.3` to `200.1.1.100` |
| External reachability | `ping www.google.de` and `ping 200.1.1.100` | Verified | 4/4 replies in the recorded test |
| Intranet HTTPS | Browser request to `https://192.168.66.200` | Verified | Reported successful from a client |
| Intranet HTTP | Browser request to `http://192.168.66.200` | Verified | Reported blocked from a client |
| VLAN-to-VLAN connectivity | Test representative clients in each VLAN | Pending | Record source, destination, and results |
| DHCP | Confirm lease and DNS option for each client VLAN | Pending | Server `10.10.130.2`; DNS `10.10.130.3` |
| EtherChannel | Verify bundle state and member failure behavior | Pending | Capture `show etherchannel summary` |
| STP | Verify Rapid PVST+ and root placement | Pending | Capture `show spanning-tree` for required VLANs |
| HSRP | Verify active/standby roles, priorities, preemption, tracking, and failover | Pending | Final priority/tracking audit remains open |
| OSPF | Verify neighbors, areas, passive interfaces, and default routes | Pending | Capture `show ip ospf neighbor` and route table |
| NAT/PAT | Verify live translations and return traffic | Pending | Capture `show ip nat translations` during a client test |
| SSH | Verify SSH v2 and authorized / unauthorized source behavior | Pending | ACL 1 and VTY settings require end-to-end tests |
| Port security | Verify 1-MAC access ports, 50-MAC AP port, and Protect action | Pending | AP-interface assignment requires final compliance review |
| Troubleshooting exercise | Restore end-to-end reachability in separate topology | Not started | Discovery and baseline tests not yet recorded |

## Recording a test

For each test, record:

1. Source device / VLAN and destination.
2. Exact command or application protocol.
3. Expected result.
4. Observed result.
5. Screenshot or CLI evidence filename.
6. Any limitations or untested paths.

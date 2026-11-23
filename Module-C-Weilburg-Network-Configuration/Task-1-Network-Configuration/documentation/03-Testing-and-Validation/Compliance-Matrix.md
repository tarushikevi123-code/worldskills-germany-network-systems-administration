# Module C Compliance Matrix

Status reflects evidence available in the current portfolio notes. **Pending** means not yet evidenced; it does not automatically mean the configuration is incorrect.

| Requirement area | Verification evidence needed | Status | Notes |
|---|---|---|---|
| Device identity and management | Hostnames, loopbacks, SSH v2, local login, banner and VTY settings | Configured in device records | Credentials intentionally redacted |
| VLANs and addressing | VLAN database, SVIs, access-port membership | Configured; operational audit pending | Check VLAN 2/4/6/8 on access switches and VLAN 130 on C1 |
| A1–A2 trunk | Trunk mode, native VLAN 10, allowed VLANs 2/4/6/8 | Configured in A1/A2 records | Capture `show interfaces trunk` |
| EtherChannel | LACP and operational bundle/member state | Configured; operational audit pending | Capture `show etherchannel summary` on all participating switches |
| Rapid PVST+ | Mode and root placement | Compliance review required | Captured C1 configuration showed `spanning-tree mode pvst`; task requires 802.1w |
| PortFast/BPDU Guard | Interface-level configuration on access ports | Compliance review required | C1 server access-port safeguards were not shown in supplied config |
| HSRP | Version, group, VIP, roles, priority, preemption and tracking | Partial / audit required | Supplied access-switch captures showed priority 115 on some groups; task specifies 105 preferred and 100 standby |
| DHCP relay | Relay address on each user SVI and live client lease | Configured; lease test pending | DHCP server `10.10.130.2` |
| OSPF | Router IDs, areas, neighbor state, user VLAN update suppression | Partial / operational recheck required | W2 showed both neighbors FULL; W1 Corporate neighbor was EXSTART in last detailed output supplied |
| WAN bandwidth | Interface bandwidth values | Configured | W1 ISP link showed 10,000 Kbit/s; W1 Corporate link 5,000 Kbit/s. Check W2 ISP relationship against task specification |
| Static default route | Next-hop default route on each WAN router | Configured in records | W1 next hop `195.198.11.2`; W2 next hop `195.198.11.6` |
| Default route propagation | OSPF default route and route-table evidence | Configured; end-to-end proof pending | Both WAN configs include `default-information originate` |
| NAT/PAT | Named ACL, inside/outside and overload, plus translation evidence | Configured; live test pending | W2 VLAN 2 ACL wildcard was corrected to `0.0.0.31`; verify current device output |
| SSH source restriction | ACL 1 applied to VTY 0–15 and connection tests | Configured; functional test pending | Source should be limited to Server VLAN |
| Port Security | Access-switch limits and Protect action | Configured in access-switch records; behavior pending | Verify WLAN AP port and MAC limit on both switches |
| Intranet ACL | ACL content, inbound placement and browser behavior | Configured and browser test reported | HTTPS succeeded; HTTP blocked |
| DNS and external endpoint | DNS response and ICMP tests | Verified for tested client | `www.google.de` resolved to simulated `200.1.1.100`; 4/4 ping replies reported |

## Final sign-off

Do not mark the whole project fully compliant until all pending and partial items have current command output or repeatable functional evidence. Keep time-specific troubleshooting outputs separate from the current final state.

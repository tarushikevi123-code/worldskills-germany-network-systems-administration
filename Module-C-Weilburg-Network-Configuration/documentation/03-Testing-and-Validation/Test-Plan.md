# Test Plan – Weilburg Enterprise Network

This test plan defines what to verify and how to record it. A command being listed here does not mean the test has passed.

## 1. Layer 2 switching

| Test | Procedure | Expected result |
|---|---|---|
| VLAN database | `show vlan brief` on switches | Required VLANs exist and access ports are in their assigned VLAN |
| Trunk | `show interfaces trunk` | A1–A2 link is an 802.1Q trunk; native VLAN 10; only VLANs 2, 4, 6 and 8 allowed |
| EtherChannel | `show etherchannel summary` | Required L2/L3 bundles are up and use LACP |
| STP mode/root | `show spanning-tree summary` and `show spanning-tree vlan <id>` | Rapid PVST+ active; A1 primary and A2 secondary root as designed |
| Access-port safeguards | `show running-config interface <interface>` | PortFast and BPDU Guard appear directly on access interfaces |

## 2. Gateway redundancy

| Test | Procedure | Expected result |
|---|---|---|
| HSRP roles | `show standby brief` | A1 active on VLANs 2 and 4; A2 active on VLANs 6 and 8 |
| HSRP tracking | Inspect SVI configuration and port-channel state | Required physical port-channel tracking is present |
| Gateway failover | Disable a selected active uplink in a controlled test | Standby assumes active role; clients retain gateway reachability |
| Recovery/preemption | Restore the link | Preferred switch resumes the active role according to configured preemption |

Record the affected VLAN, original active device, failure trigger, convergence and recovery behavior.

## 3. Routing

| Test | Procedure | Expected result |
|---|---|---|
| OSPF neighbors | `show ip ospf neighbor` | Required adjacencies reach FULL |
| OSPF areas | `show ip ospf interface brief` | Site links Area 0; corporate links Area 2 |
| Default route | `show ip route 0.0.0.0` | Default route learned/installed as expected |
| Route propagation | `show ip route` on devices | Required site and corporate routes are present |
| WAN failover | Controlled failure of one WAN path | Alternate path remains usable and routes reconverge |

## 4. Network services

| Test | Procedure | Expected result |
|---|---|---|
| DHCP | `ipconfig /all` or Packet Tracer client IP configuration | Correct address, mask, HSRP gateway and DNS server in each user VLAN |
| DNS | `nslookup www.google.de` | DNS server 10.10.130.3 returns the configured simulated endpoint |
| NAT/PAT | Generate traffic then run `show ip nat translations` | Expected internal source is translated to WAN interface address |
| External reachability | Ping the simulated external endpoint | Replies received from the configured simulated endpoint |

## 5. Security

| Test | Procedure | Expected result |
|---|---|---|
| SSH allowed | Connect from a Server VLAN host | SSH login succeeds |
| SSH denied | Connect from a non-Server VLAN host | VTY source ACL denies access |
| Port Security | Test an additional MAC on a protected access port, if safe in the lab | Violation action matches Protect; interface remains forwarding |
| Intranet HTTPS | Browse to `https://192.168.66.200` | HTTPS succeeds |
| Intranet HTTP | Browse to `http://192.168.66.200` | HTTP is denied |

## 6. Test record template

For each run, record:

- Test ID and date.
- Source device and VLAN.
- Destination and protocol.
- Exact command or action.
- Expected result.
- Observed output/result.
- Screenshot path.
- Pass, fail or pending status.
- Notes or limitations.

# Evidence Folder

Use this folder for screenshots exported from the final Cisco Packet Tracer session. Keep evidence focused on observed configuration and repeatable test results.

Recommended evidence groups:

1. **Topology:** final logical Packet Tracer view showing all routers, LANs, serial links and the external server. Use your own Packet Tracer screenshot; do not redistribute the official task PDF artwork unless you have permission.
2. **Interfaces:** `show ip interface brief` for each router and `show interfaces status` / `show vlan brief` for relevant switches.
3. **OSPF:** `show ip ospf neighbor`, `show ip ospf interface brief`, and `show ip route ospf` from representative routers.
4. **ACLs:** `show access-lists` and `show ip interface <interface>` for ACL attachment and direction.
5. **Connectivity:** PC/server `ipconfig` plus successful `ping` output, with the source device visible in the screenshot.
6. **Final state:** screenshots showing the repaired network and final end-to-end tests.

Suggested filenames:

```text
01-final-topology.png
02-serial-interface-addressing.png
03-ospf-neighbors-weil-2.png
04-ospf-neighbors-wan.png
05-routing-table-wan.png
06-vlan-table-weil-switch.png
07-pc-berlin-pings.png
08-pc-hannover-pings.png
09-pc-weil-1-pings.png
10-pc-weil-3-pings.png
11-lap-weil-ping.png
12-server-ping-tests.png
```

Do not include passwords, password hashes, private access tokens, personal account details or unrelated desktop information in screenshots. Crop screenshots so the CLI output and device identity remain readable.
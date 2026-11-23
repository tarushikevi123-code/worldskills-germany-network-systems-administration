# Troubleshooting Log

This log records the faults observed during the Packet Tracer investigation, the corrective actions taken and the verification evidence captured. It is ordered by the sequence of the investigation rather than by OSI layer.

## 1. Initial investigation approach

The recovery process followed a repeatable sequence:

1. Inspect device power and physical links.
2. Check interface status with `show ip interface brief` and `show interfaces status`.
3. Inspect the running configuration of the affected interface.
4. Verify IP addressing and subnet membership against the topology.
5. Check switchport VLAN assignment.
6. Inspect OSPF participation, neighbors and routing tables.
7. Review ACL contents and interface bindings.
8. Retest the same destination after each change.

Commands used frequently:

```cisco
show ip interface brief
show interfaces status
show interfaces <interface>
show running-config
show running-config interface <interface>
show vlan brief
show interfaces trunk
show ip ospf neighbor
show ip ospf interface brief
show ip route
show ip route ospf
show ip protocols
show access-lists
show ip interface <interface>
show ip arp
```

## 2. Incident log

### Incident 01 – Server-Berlin powered off

**Device:** Server-Berlin / S-Berlin access switch  
**Symptom:** The server-connected switch port was red and remained down after `no shutdown` was attempted.  
**Checks:** `show ip interface brief`, `show interfaces fastEthernet 0/24` and `show port-security interface fastEthernet 0/24`. Port security was disabled and the violation count was zero.  
**Root cause:** The server itself was powered off.  
**Fix:** Powered on Server-Berlin.  
**Verification:** The server endpoint became available; the switch port issue was not a port-security violation.

### Incident 02 – S-Berlin router uplink administratively disabled

**Device/interface:** S-Berlin Gi0/1  
**Symptom:** Uplink to R-Berlin was down.  
**Root cause:** Interface was administratively disabled.  
**Fix:**

```cisco
configure terminal
interface gigabitEthernet 0/1
no shutdown
```

**Verification:** Packet Tracer reported both the link and line protocol changing to up.

### Incident 03 – Speed mismatch between S-Server and R-Server

**Devices/interfaces:** S-Server Gi0/1 and R-Server Gi0/0/0  
**Observed settings:** S-Server Gi0/1 was configured for `speed 1000`; R-Server Gi0/0/0 was configured for `speed 100`.  
**Root cause:** The two ends had incompatible forced speed settings.  
**Fix on R-Server:**

```cisco
configure terminal
interface gigabitEthernet 0/0/0
speed 1000
```

**Verification:** Packet Tracer logged `%LINEPROTO-5-UPDOWN` with the line protocol changing to up. The observed trigger for the state change was correcting the speed.

### Incident 04 – R-Weil-1 Ethernet duplex setting

**Device/interface:** R-Weil-1 Gi0/0/0  
**Observed setting:** `duplex half` while the interface was expected to use full duplex.  
**Fix:**

```cisco
configure terminal
interface gigabitEthernet 0/0/0
duplex full
```

**Verification:** Packet Tracer reported the line protocol changing to up. The opposite endpoint's final duplex setting was not captured, so this is documented as a corrected forced-duplex setting rather than an independently measured two-sided mismatch.

### Incident 05 – S-Weil-1 router uplink administratively disabled

**Device/interface:** S-Weil-1 Gi0/1  
**Symptom:** `show interfaces status` showed the interface as `disabled`.  
**Fix:**

```cisco
configure terminal
interface gigabitEthernet 0/1
no shutdown
```

**Verification:** The interface became `connected`; the switch reported auto-negotiated full duplex and 100 Mbps.

### Incident 06 – R-Weil-2 serial IP addresses assigned to the wrong interfaces

**Device:** R-Weil-2  
**Symptom:** Serial links were up/up and included in OSPF, but the expected neighbor relationships did not form.  
**Initial mapping:** Se0/1/0 had `10.10.0.241`, Se0/1/1 had `10.10.0.249`, and Se0/2/0 had `10.10.0.238`.  
**Physical topology mapping:**

| Interface | Correct peer | Correct IP |
|---|---|---:|
| Se0/2/0 | R-Weil-WAN | `10.10.0.238/30` |
| Se0/1/1 | R-Server | `10.10.0.241/30` |
| Se0/1/0 | R-Weil-1 | `10.10.0.249/30` |

**Fix:** Reassigned Se0/1/0 to `10.10.0.249/30` and Se0/1/1 to `10.10.0.241/30`. Se0/2/0 remained `10.10.0.238/30`.  
**Verification:** R-Weil-2 logged transitions to FULL and `show ip ospf neighbor` displayed:

- Neighbor `5.5.5.5` at `10.10.0.250` on Se0/1/0 – `FULL`.
- Neighbor `6.6.6.6` at `10.10.0.242` on Se0/1/1 – `FULL`.

### Incident 07 – Missing OSPF network statement on R-Server

**Device:** R-Server  
**Symptom:** The R-Server-to-R-Weil-1 serial link did not form an OSPF adjacency.  
**Observed interface:** Se0/1/0 `10.10.0.245/30`, belonging to `10.10.0.244/30`.  
**Fix:** Added the exact network match under OSPF process 10:

```cisco
router ospf 10
network 10.10.0.244 0.0.0.3 area 0
```

**Verification:** R-Server logged neighbor `5.5.5.5` on Se0/1/0 moving from LOADING to FULL. Its neighbor table also showed R-Weil-2 (`4.4.4.4`) FULL on Se0/1/1.

### Incident 08 – Missing OSPF network statement and custom timers on R-Weil-WAN

**Device:** R-Weil-WAN  
**Observed interface:** Se0/1/0 `10.10.0.237/30`, belonging to `10.10.0.236/30`.  
**Findings:** The OSPF process did not include the serial subnet. The interface also had custom Hello/Dead intervals of 20/80, while the neighbor was using the default timers.  
**Fixes:**

```cisco
interface serial 0/1/0
no ip ospf hello-interval 20
no ip ospf dead-interval 80
!
router ospf 10
network 10.10.0.236 0.0.0.3 area 0
```

**Verification:** R-Weil-WAN logged neighbor `4.4.4.4` on Se0/1/0 moving from LOADING to FULL. Its neighbor table showed R-Weil-2 FULL at `10.10.0.238`.

### Incident 09 – Incorrect shared-LAN address on R-Hannover-1

**Device/interface:** R-Hannover-1 Gi0/0/1  
**Incorrect address:** `10.1.1.19/29`  
**Expected shared subnet:** `10.10.1.16/29`  
**Fix:**

```cisco
configure terminal
interface gigabitEthernet 0/0/1
ip address 10.10.1.19 255.255.255.248
no shutdown
```

**Verification:** OSPF logged neighbor `9.9.9.9` moving to FULL. The interface showed BDR in `show ip ospf interface brief`.

### Incident 10 – Incorrect Berlin LAN address

**Device/interface:** R-Berlin Gi0/0/0  
**Fix:** Corrected the LAN interface to `192.168.11.1 255.255.255.0` and enabled it with `no shutdown`.  
**Verification:** `show ip interface brief` showed Gi0/0/0 as `192.168.11.1`, up/up.

### Incident 11 – R-Berlin shared router interface incorrectly passive

**Device/interface:** R-Berlin Gi0/0/1 (`10.10.1.18/29`)  
**Symptom:** The shared Ethernet interface was passive even though it connects to R-Weil-WAN and R-Hannover, both OSPF routers.  
**Fix:**

```cisco
router ospf 10
no passive-interface gigabitEthernet 0/0/1
```

Gi0/0/0, which faces the Berlin end-host LAN, remained passive.  
**Verification:** R-Berlin logged both adjacencies moving to FULL:

- R-Weil-WAN, RID `9.9.9.9`, address `10.10.1.17`, observed as DR.
- R-Hannover-1, RID `7.7.7.7`, address `10.10.1.19`, observed as BDR.

### Incident 12 – R-Hannover ACL blocked traffic sourced from Berlin

**Device/interface:** R-Hannover-1 Gi0/0/1, inbound  
**ACL 10:**

```cisco
access-list 10 deny 192.168.11.0 0.0.0.255
access-list 10 permit any
```

**Effect:** The standard ACL denied traffic sourced from the Berlin LAN when it entered R-Hannover through the shared router Ethernet segment. This included ICMP traffic from Berlin.  
**Fix:** Removed the ACL binding but retained the ACL definition:

```cisco
interface gigabitEthernet 0/0/1
no ip access-group 10 in
```

**Verification:** Subsequent cross-LAN tests from PC-Berlin and PC-Hannover succeeded. `show access-lists` still displayed ACL 10 because only the interface binding was removed.

### Incident 13 – R-Server ACL blocked ICMP

**Device/interface:** R-Server Gi0/0/0, inbound  
**ACL 100:**

```cisco
access-list 100 deny icmp any any
```

The ACL was applied inbound on the interface connected to the Server LAN. With no subsequent permit entry, the implicit deny also prevented other unmatched IP traffic.  
**Fix:** Removed ACL 100:

```cisco
no access-list 100
```

**Verification:** `show access-lists` returned no ACL entries after removal. Later tests from client LANs to the Server-LAN-side address `192.168.33.100` succeeded. The final server-side interface addressing should still be checked against the topology note in `TOPOLOGY-AND-ADDRESSING.md`.

### Incident 14 – PC-Hannover configured in the wrong /25 subnet

**Host:** PC-Hannover  
**Initial address:** `192.168.22.200/25`, while its gateway was `192.168.22.1/25`. The PC address belongs to `192.168.22.128/25`; the gateway belongs to `192.168.22.0/25`.  
**Fix:** Changed PC-Hannover to `192.168.22.57/25`, default gateway `192.168.22.1`.  
**Verification:** Ping to `192.168.22.1` succeeded 4/4, and ping to `203.0.113.100` succeeded 4/4 with 0% loss.

### Incident 15 – Weilburg upper LAN host, mask and VLAN mismatch

**LAN:** `192.168.44.0/26`  
**Findings:** A PC initially used `192.168.44.195/26`, which is outside the `/26` subnet, and the gateway was mistyped as `192.169.44.1`. R-Weil-1 Gi0/0/0 was also shown with `/27`, while the topology specifies `/26`. On S-Weil-1, Fa0/1 was in VLAN 10 while the other relevant ports and router uplink were in VLAN 1.  
**Corrections recorded:**

- PC address changed to `192.168.44.20/26`, gateway `192.168.44.1`.
- R-Weil-1 Gi0/0/0 corrected to `192.168.44.1/26`.
- S-Weil-1 Fa0/1 configured as access VLAN 1; the unused VLAN 10 was removed.

**Verification:** PC-Weil-1 successfully pinged `192.168.44.1` with 4/4 replies and 0% loss. Later cross-LAN tests from PC-Weil-1 also succeeded.

### Incident 16 – Weilburg-1 gateway address typo

**Device/interface:** R-Weil-1 Gi0/0/1  
**Incorrect address:** `192.169.55.1/27`  
**Correct address:** `192.168.55.1/27`  
**Root cause:** Second octet typo (`169` instead of `168`).  
**Verification:** PC-Weil-3 (`192.168.55.26/27`) successfully pinged its gateway `192.168.55.1` and the external server `203.0.113.100`, each with 4/4 replies and 0% loss.

## 3. ACLs reviewed

| Device | ACL | Rule / application observed | Action taken |
|---|---|---|---|
| R-Berlin | Extended ACL 100 | Denied TCP/HTTP (`eq www`) to host `192.168.11.100`; permitted other IP traffic; applied inbound on Gi0/0/1 | Retained; this rule does not deny ICMP |
| R-Hannover-1 | Standard ACL 10 | Denied source `192.168.11.0/24`; applied inbound on Gi0/0/1 | Removed interface binding; ACL definition retained |
| R-Server | Extended ACL 100 | `deny icmp any any`; applied inbound on Gi0/0/0 | Removed ACL 100 |
| R-Weil-1 | ACL 101 reference | `ip access-group 101 in` was visible on Gi0/0/0, while `show access-lists` displayed no entries | No confirmed root cause or removal was recorded; treat as an observed configuration reference, not a proven repair |

## 4. OSPF neighbor evidence captured

The following neighbor states were visible after the relevant fixes:

### R-Weil-2

| Neighbor RID | Neighbor address | Local interface | State |
|---|---:|---|---|
| `5.5.5.5` (R-Weil-1) | `10.10.0.250` | Se0/1/0 | FULL |
| `6.6.6.6` (R-Server) | `10.10.0.242` | Se0/1/1 | FULL |

### R-Server

| Neighbor RID | Neighbor address | Local interface | State |
|---|---:|---|---|
| `4.4.4.4` (R-Weil-2) | `10.10.0.241` | Se0/1/1 | FULL |
| `5.5.5.5` (R-Weil-1) | `10.10.0.246` | Se0/1/0 | FULL |

### R-Weil-WAN

| Neighbor RID | Neighbor address | Local interface | State |
|---|---:|---|---|
| `4.4.4.4` (R-Weil-2) | `10.10.0.238` | Se0/1/0 | FULL |
| `7.7.7.7` (R-Hannover-1) | `10.10.1.19` | Gi0/0/0 | FULL/BDR |
| `8.8.8.8` (R-Berlin) | `10.10.1.18` | Gi0/0/0 | FULL/DROTHER |

### R-Berlin

| Neighbor RID | Neighbor address | Local interface | State |
|---|---:|---|---|
| `7.7.7.7` (R-Hannover-1) | `10.10.1.19` | Gi0/0/1 | FULL/BDR |
| `9.9.9.9` (R-Weil-WAN) | `10.10.1.17` | Gi0/0/1 | FULL/DR |

## 5. Routing-table evidence

A captured `show ip route ospf` output on R-Weil-WAN showed OSPF-learned routes for:

- `10.10.0.240/30`, via `10.10.0.238`.
- `10.10.0.244/30`, via `10.10.0.238`.
- `10.10.0.248/30`, via `10.10.0.238`.
- `192.168.11.0/24`, via `10.10.1.18`.
- `192.168.22.0/25`, via `10.10.1.19`.
- An intermediate route entry for `192.168.44.0/27`, via `10.10.0.238`; this was captured before the later correction of R-Weil-1 Gi0/0/0 to `/26`, so it is not treated as the final route mask.

## 6. Final outcome

The operator reported that the task objective was achieved: all PCs and servers could communicate with one another and reach the external server using IPv4 ICMP. Captured endpoint tests are summarized in [CONNECTIVITY-VERIFICATION.md](CONNECTIVITY-VERIFICATION.md).

The captured session does not include the final full running configurations of every router, a full server-to-server ping matrix, or the final R-Server Server-LAN interface output. These are listed as evidence follow-ups rather than silently assumed.
# Network Services and Security Implementation

## 1. DHCP and DNS

The server VLAN is `10.10.130.0/29`. The supplied task identifies:

| Service | Address |
|---|---|
| DHCP | `10.10.130.2` |
| DNS | `10.10.130.3` |

The DHCP server provides scopes for user VLANs 2, 4, 6 and 8. DHCP relay must be configured on the user VLAN gateway SVIs, forwarding requests to `10.10.130.2`.

Client verification should record the assigned IPv4 address, subnet mask, default gateway and DNS server for at least one client in each VLAN.

## 2. NAT/PAT

Each WAN router uses:

- Named standard ACL `vlans` to select internal source networks.
- `ip nat inside` on the site-facing interface.
- `ip nat outside` on the ISP-facing Serial0/1/0 interface.
- PAT overload using the ISP-facing interface address.

Expected inside networks:

| Network | Wildcard |
|---|---|
| `10.10.2.0/27` | `0.0.0.31` |
| `10.10.4.0/28` | `0.0.0.15` |
| `10.10.6.0/26` | `0.0.0.63` |
| `10.10.8.0/29` | `0.0.0.7` |
| `10.10.130.0/29` | `0.0.0.7` |

A correction was reported for W2's VLAN 2 ACL wildcard: the original captured entry `0.0.0.3` was changed to `0.0.0.31`. The updated W2 text record reflects the correction. Verify the live device ACL before considering it fully confirmed.

Useful verification:

```cisco
show access-lists vlans
show ip nat statistics
show ip nat translations
```

## 3. Secure device management

The device configuration records show the following management design:

- Local user authentication.
- SSH version 2.
- Local console authentication.
- VTY lines 0–15 use local login and SSH-only transport.
- Standard numbered ACL 1 restricts VTY access to the Server VLAN subnet `10.10.130.0/29`.
- Domain name `skill39.org`.
- MOTD warning banner.

Credential values and password hashes are intentionally omitted from this documentation.

Functional verification should test SSH from an authorized Server VLAN source and from a source outside the permitted subnet.

## 4. Port Security

The task applies Port Security to access switches only. The specified maximum is one MAC address on regular access ports and 50 on the WLAN access-point port, with the violation action set to Protect.

Verification:

```cisco
show port-security
show port-security interface gigabitEthernet1/0/21
show port-security address
```

Confirm the AP interface and its configured MAC limit on both access switches against the actual topology.

## 5. Intranet access policy

The named extended ACL `intranet` on both WAN routers applies inbound on GigabitEthernet0/0/0.

| Destination | Protocol | Expected action |
|---|---|---|
| `192.168.66.200` | TCP/443 (HTTPS) | Permit |
| `192.168.66.200` | TCP/80 (HTTP) | Deny |
| Any other IP traffic | IP | Permit |

The supplied client browser tests reported HTTPS success and HTTP blocked. This is a functional observation for the tested client and path.

Verify ACL contents, placement, counters and behavior:

```cisco
show access-lists intranet
show ip interface gigabitEthernet0/0/0
```

# Switching and VLAN Implementation

## 1. VLAN segmentation

The access layer separates HR, Sales, Engineering and WLAN clients into dedicated VLANs. The server network is terminated on the Layer 3 core switch.

| VLAN | Role | Subnet |
|---:|---|---|
| 2 | HR | `10.10.2.0/27` |
| 4 | SALES | `10.10.4.0/28` |
| 6 | ENGINEERING | `10.10.6.0/26` |
| 8 | WLAN | `10.10.8.0/29` |
| 130 | SERVER | `10.10.130.0/29` |

## 2. Access-port allocation

The task's access-port allocation on Weil_A1 and Weil_A2 is:

| Interfaces | VLAN | Department |
|---|---:|---|
| Gi1/0/3–8 | 2 | HR |
| Gi1/0/9–16 | 4 | SALES |
| Gi1/0/17–20 | 6 | ENGINEERING |
| Gi1/0/21–22 | 8 | WLAN |

On Weil_C1, Gi1/0/3–20 connect server devices in VLAN 130. Verify fixed access mode on end-device ports and the correct VLAN presence with operational commands.

## 3. Trunk and EtherChannel design

- A1 and A2 are interconnected using an L2 EtherChannel.
- The trunk uses IEEE 802.1Q, native VLAN 10, and allows only VLANs 2, 4, 6 and 8.
- Each access switch has a separate L3 EtherChannel to C1.
- LACP is the required negotiation protocol for both L2 and L3 EtherChannels.

Useful verification commands:

```cisco
show etherchannel summary
show interfaces trunk
show interfaces port-channel
show vlan brief
```

## 4. Spanning Tree

The required switch mode is Rapid PVST+ (802.1w). A1 is the intended primary root bridge and A2 the secondary root bridge for the user VLANs.

End-device access interfaces should have PortFast and BPDU Guard configured directly on the interfaces. The BPDU Guard action should disable the port if a BPDU is received.

Verification:

```cisco
show spanning-tree summary
show spanning-tree vlan 2
show spanning-tree vlan 4
show spanning-tree vlan 6
show spanning-tree vlan 8
```

## 5. HSRP gateway redundancy

The user VLAN SVIs use HSRP version 1, with the HSRP group number matching the VLAN ID.

| VLAN | Virtual IP | Intended Active | Intended Standby |
|---:|---|---|---|
| 2 | `10.10.2.30` | Weil_A1 | Weil_A2 |
| 4 | `10.10.4.14` | Weil_A1 | Weil_A2 |
| 6 | `10.10.6.62` | Weil_A2 | Weil_A1 |
| 8 | `10.10.8.6` | Weil_A2 | Weil_A1 |

The design specifies priority 105 on the preferred active switch and 100 on the standby switch, preemption, and tracking of the physical port-channel uplink to C1.

Verification:

```cisco
show standby brief
show standby
```

A failover test should be documented with the active role before failure, the event triggered, the new active role, and recovery/preemption behavior. Do not mark failover as tested from configuration alone.

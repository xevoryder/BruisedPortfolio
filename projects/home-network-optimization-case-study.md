# Home Network Optimization & Gigabit Ethernet Restoration

## Project Snapshot

| Item | Detail |
|---|---|
| Environment | Home LAN supporting Windows Server, Ubuntu Server, WireGuard, Jellyfin, and other self-hosted services |
| Router | ASUS ROG Rapture GT-AC5300 |
| Primary work | DHCP reservation, role separation, structured Layer 1–3 troubleshooting |
| Validated result | Stable server addressing and Gigabit Ethernet restored across two affected network paths |

## Objective

Create a predictable home-network foundation for server-hosted services and diagnose two wired paths that were negotiating at 100 Mbps instead of their expected Gigabit rate.

The work involved two related goals:

1. Keep infrastructure endpoints reachable at consistent LAN addresses.
2. Find the physical cause of the unexpected link-speed limit without replacing equipment blindly.

---

## Case 1 — Stable Server Addressing

### Observed Risk

A server receiving a different address from DHCP can break bookmarks, service references, routing assumptions, and troubleshooting baselines.

### Corrective Action

I mapped the server's network interface to a consistent address through a router-managed DHCP reservation. This kept address management centralized while ensuring the server received the same LAN address after reconnects or reboots.

### Role Separation

| Component | Responsibility |
|---|---|
| Router | DHCP, default gateway, LAN addressing, NAT, port forwarding, and network management |
| Servers | WireGuard, media, storage, application, and other hosted services |

### Validation

The server retained its expected address and local services remained reachable through consistent connection details.

### Takeaway

Stable addressing reduces variables. When a service fails, the operator can investigate the service or network path without first wondering whether the endpoint moved.

---

## Case 2 — Gigabit Links Negotiating at 100 Mbps

### Expected Behavior

The affected wired network paths should negotiate at 1 Gbps.

### Actual Behavior

Two paths were limited to 100 Mbps even though the connected switching equipment and endpoints supported Gigabit Ethernet.

### Diagnostic Method

I treated the link as a chain and isolated each component instead of assuming the switches were defective:

1. Checked the connected endpoints and reported link rate.
2. Isolated the switches from the permanent cabling.
3. Swapped known-good patch cables into the path.
4. Compared behavior before and after the wall runs.
5. Narrowed the shared failure point to the wall-jack terminations.
6. Inspected and corrected the failed keystone jacks.

### Root Cause

The wall jacks had failed termination points. Gigabit Ethernet requires all four twisted pairs, while a damaged pair can allow a link to fall back to 100 Mbps instead of failing completely.

### Corrective Action

I replaced the defective keystone termination points and revalidated the complete paths.

### Result

Both affected network paths returned to Gigabit Ethernet connectivity.

### Takeaway

A working link is not necessarily a healthy link. Layer 1 faults can appear as a performance limitation, so switches, patch cables, permanent cabling, termination points, and endpoints must be isolated systematically.

---

## Troubleshooting Layers Used

| Layer | Checks |
|---|---|
| Physical | Patch cables, wall runs, RJ45/keystone terminations, endpoint connections |
| Data link | Negotiated Ethernet rate and switch-port behavior |
| Network | Stable addressing, gateway responsibilities, and service reachability |

---

## Skills Demonstrated

- Layer 1–3 troubleshooting
- Ethernet link-speed diagnosis
- Cat 5e/Cat 6 termination work
- RJ45 and keystone troubleshooting
- Switch and endpoint isolation
- DHCP reservation planning
- Router/server role separation
- End-to-end validation
- Technical documentation

## Professional Relevance

This project maps directly to network support, field support, technical support, and junior infrastructure work. It demonstrates the ability to resist assumptions, isolate a fault across several components, repair the actual failure point, and verify the restored service level.

[Back to portfolio](../README.md)

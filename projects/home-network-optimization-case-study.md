# Home Network Optimization Case Study

## Project Type
Networking fundamentals / router configuration / homelab reliability

## Mission Objective
Stabilize the home network so server-hosted services and VPN access could function more reliably.

The goal was to reduce confusion caused by changing IP addresses and clarify which parts of the network should be handled by the router versus the server.

---

## Environment

| Component | Details |
|---|---|
| Router | ASUS ROG Rapture GT-AC5300 |
| Server Role | Ubuntu home server / services host |
| Services Supported | WireGuard, Jellyfin, OpenClaw, CasaOS-related access |
| Primary Concern | Stable addressing and service reliability |

---

## Failure Index

1. Server IP could change dynamically
2. DHCP reservation needed clarification
3. Router and server responsibilities were blending together
4. VPN setup required stable internal addressing
5. Local service access needed predictable network behavior

---

## Issue 1 — Dynamic IP Address Concern

### Problem
If the server receives a different IP from DHCP, services become harder to reach. Bookmarks, configs, VPN routing assumptions, and local access paths can break.

### Root Cause
Default DHCP behavior can assign different addresses over time unless a reservation is created.

### Fix Concept
Use DHCP reservation on the router to map the server’s MAC address to a consistent internal IP.

### Why This Matters
Static addressing makes troubleshooting cleaner. If the server is always at the same IP, failures are easier to isolate.

### Lesson Learned
Stable infrastructure starts with stable addressing.

---

## Issue 2 — DHCP Reservation Confusion

### What Needed To Be Understood
A DHCP reservation is not the same as randomly setting an IP on a device.

The router still manages DHCP, but it always gives the same IP to a specific device.

### Mental Model

```text
Device MAC Address
        ↓
Router DHCP Reservation
        ↓
Same IP assigned every time
        ↓
Services stay reachable
```

### Lesson Learned
DHCP reservation keeps centralized control at the router while still giving server-like IP stability.

---

## Issue 3 — Router vs Server Responsibilities

### Problem
There was confusion around what the router should handle versus what the server should handle.

### Clean Separation

| Role | Responsibility |
|---|---|
| Router | DHCP, LAN addressing, gateway, port forwarding, Wi-Fi/network management |
| Server | Hosted applications, VPN service, media server, gateway apps, local services |

### Lesson Learned
When troubleshooting, define ownership. If responsibilities overlap mentally, troubleshooting becomes messy.

---

## Issue 4 — VPN and Service Reliability

### Why Network Stability Affected WireGuard
VPN configs often assume a specific internal addressing scheme. If the server changes IP, access assumptions can break.

### Why Network Stability Affected Jellyfin/OpenClaw
Local services become easier to reach when the server stays in one known location on the LAN.

### Lesson Learned
Networking problems often show up as application problems. Fixing the network foundation prevents app-level confusion.

---

## Before / After

| State | Description |
|---|---|
| Before | Services depended on a home network that could shift addresses and create confusion. |
| After | Network planning focused on stable server IP, cleaner router/server role separation, and better troubleshooting flow. |

---

## Skills Demonstrated

- Router configuration concepts
- DHCP reservation planning
- LAN addressing
- VPN support planning
- Service reliability thinking
- Network troubleshooting
- Systems documentation

---

## Professional Relevance

This project maps to IT support and network support because it required understanding:

- DHCP behavior
- Static vs reserved addressing
- Service availability
- Router responsibilities
- Server responsibilities
- How network instability can affect application access

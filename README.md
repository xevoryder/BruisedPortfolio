# BruisedPortfolio

## Tristan Kania — Network & Systems Support

Hands-on technical portfolio documenting real work with Windows and Ubuntu Server infrastructure, WireGuard VPN, RAID/NAS storage, wired networking, and self-hosted services.

I approach technical problems as an operator: define the expected behavior, isolate the failing layer, change one variable at a time, validate the result, and document what changed. My background also includes five years of operations leadership, which strengthened my ability to coordinate people, prioritize work, and stay methodical under pressure.

> **Scope note:** These projects were built and troubleshot in a self-directed homelab. They demonstrate practical ability without presenting the environment as employer-managed production infrastructure.

---

## Featured Projects

| Project | Evidence |
|---|---|
| [WireGuard VPN Server](./projects/wireguard-vpn-operator-log.md) | Deployed WireGuard on Ubuntu Server 24.04, onboarded a mobile peer, resolved permissions and configuration failures, and validated the traffic path rather than relying only on connection status. |
| [Home Network Optimization & Gigabit Restoration](./projects/home-network-optimization-case-study.md) | Stabilized server addressing with DHCP reservations and restored Gigabit Ethernet across two affected paths by isolating switches, patch cables, wall jacks, and endpoints before replacing failed keystone terminations. |
| [Linux Application Gateway Troubleshooting](./projects/openclaw-gateway-operator-log.md) | Used `systemctl --user`, service logs, JSON inspection, origin controls, and permission analysis to diagnose a multi-layer application gateway deployment. |
| [Jellyfin Media Server](./projects/jellyfin-media-server-case-study.md) | Designed a self-hosted media service around practical storage, LAN access, client integration, and future expansion requirements. |

---

## Technical Environment

| Area | Technologies and Practices |
|---|---|
| Networking | TCP/IP, IPv4 subnetting/VLSM, VLAN concepts, DHCP, DNS, NAT, port forwarding, WireGuard, routing and switching fundamentals, Layer 1–3 troubleshooting |
| Systems | Windows Server, Ubuntu Server 24.04, Linux CLI, SSH, systemd, Docker, CasaOS, self-hosted services |
| Infrastructure | Rack-mounted hardware, RAID/NAS storage, SMB/NFS concepts, BIOS/UEFI, Cat 5e/Cat 6, RJ45 connectors, keystone terminations |
| Tools | Cisco Packet Tracer, ASUS ROG Rapture GT-AC5300, GitHub, Microsoft Word and Excel |

---

## Selected Outcomes

- Restored Gigabit Ethernet connectivity across two network paths by identifying and replacing failed keystone termination points.
- Enabled remote access to internal services through a WireGuard VPN with mobile peer onboarding and routing validation.
- Built and maintained rack-mounted Windows Server and Ubuntu Server infrastructure supporting centralized storage and self-hosted services.
- Resolved service failures involving Linux permissions, missing dependencies, malformed configuration, secure-origin controls, and systemd behavior.

---

## Troubleshooting Method

1. Define the expected behavior.
2. Capture the actual failure.
3. Identify the affected layer: hardware, cabling, network, operating system, service, application, configuration, or permissions.
4. Test one variable at a time.
5. Apply the smallest reasonable correction.
6. Validate the outcome end to end.
7. Document the problem, root cause, fix, and lesson learned.

[View the full troubleshooting framework](./docs/troubleshooting-method.md)

---

## Professional Direction

I am pursuing opportunities in network and systems support, technical support, junior infrastructure operations, and network-engineer trainee roles. I am actively studying for the **Cisco CCNA** and expanding this portfolio as I complete additional labs and infrastructure projects.

[LinkedIn](https://www.linkedin.com/in/tristan-kania-a57aa3259)

## BruisedPortfolio

## Tristan Kania — IT Operations, Networking, CRM Systems & Investigative Technical Analysis

Hands-on technical portfolio focused on Linux systems, networking, CRM operations, self-hosted infrastructure, open-source research workflows, and structured troubleshooting.

This portfolio documents real systems I have built, problems I encountered, how I diagnosed them, and the solutions I implemented. My goal is to show practical technical ability through documented work: not just finished projects, but the thinking process behind fixing systems, organizing information, and turning messy problems into repeatable workflows.

My background combines operations leadership, Salesforce/Litify CRM case workflows, legal intake analysis, incident documentation, homelab infrastructure, and open-source research methods. That combination gives me a strong foundation for IT operations, technical support, CRM systems support, junior systems administration, network support, and investigative analyst roles..

---

## Core Focus Areas

- Linux server administration
- Networking fundamentals
- VPN deployment and troubleshooting
- CRM / Salesforce-based workflow operations
- Home server infrastructure
- Technical documentation
- Root-cause analysis
- System-level problem solving

---

## Featured Projects

### WireGuard VPN Server

Built and configured a secure WireGuard VPN server using Ubuntu Server 24.04 to enable remote access into a private home network.

**What this project demonstrates:**

- Linux CLI usage
- VPN configuration
- Peer setup and mobile onboarding
- QR code provisioning
- Routing troubleshooting
- Permission issue diagnosis
- Full-tunnel traffic validation

**Problems solved:**

- Permission denied errors inside `/etc/wireguard/`
- QR code generation issues
- Broken or malformed mobile peer configs
- VPN connection working but traffic not routing properly
- Confusion between “connected” and “actually routing through tunnel”

[View Project](./projects/wireguard-operator-log.md)

---

### OpenClaw Gateway Setup & Troubleshooting

Worked through OpenClaw gateway setup, service behavior, configuration issues, and remote access problems while integrating local AI/control tooling into a home server environment.

**What this project demonstrates:**

- Linux service troubleshooting
- Systemd user service checks
- Config file debugging
- Gateway / control UI troubleshooting
- Secure context and origin restriction analysis
- Local service access validation
- Iterative problem solving under unclear documentation

**Problems investigated:**

- Gateway service status and runtime behavior
- Control UI access restrictions
- Origin not allowed errors
- Device identity / HTTPS secure context issues
- Configuration syntax and allowed origin problems
- Service logs and restart behavior

[View Project](./projects/openclaw-operator-log.md)

---

### Home Media Server — Jellyfin

Designed and deployed a private home media server using Jellyfin with local streaming access and future expansion in mind.

**What this project demonstrates:**

- Self-hosted media server setup
- Local network service access
- Storage planning
- Client-device integration
- Server vs NAS architecture decisions
- Practical home infrastructure planning

**Problems solved:**

- Determining whether a separate NAS was required
- Organizing storage for media access
- Understanding local vs remote access requirements
- Planning for Apple TV playback
- Designing for future scalability without unnecessary hardware spending

[View Project](./projects/media-detailed.md)

---

### Home Network Optimization

Configured and troubleshot home network settings to support stable server access, VPN routing, and consistent internal addressing.

**What this project demonstrates:**

- Router configuration
- DHCP reservation planning
- Internal IP stability
- VPN/server role separation
- Local network troubleshooting
- Practical networking fundamentals

**Problems solved:**

- Server IP changing dynamically
- Confusion around DHCP reservation
- Router vs server responsibility overlap
- VPN routing concerns
- Local network reliability issues

[View Project](./projects/network-detailed.md)

---

## Research & Investigation Workflows

In addition to infrastructure projects, I also study structured research and investigative workflows. This includes open-source intelligence concepts, case triage methods, documentation discipline, source evaluation, and analytical reporting.

These skills connect directly to both technical and operational environments because many real-world IT issues require more than running commands. They require gathering information, verifying facts, documenting evidence, identifying patterns, and explaining findings clearly.

### Skills Demonstrated

- Open-source research methods
- Source evaluation
- Case triage and issue classification
- Timeline reconstruction
- Evidence organization
- Pattern recognition
- Documentation and reporting
- Risk-aware thinking
- Clear written summaries for decision-makers
  
---
## Technical Skills Demonstrated

### Systems

- Ubuntu Server 24.04
- Linux CLI
- Systemd user services
- File permissions
- Config file editing
- Service status checks
- Log review

### Networking

- WireGuard VPN
- DHCP reservation
- Local IP addressing
- Routing concepts
- Port forwarding concepts
- LAN service access
- Router configuration

### Platforms & Tools

- GitHub
- Salesforce / Litify CRM
- Jellyfin
- OpenClaw
- ASUS ROG Rapture GT-AC5300
- Microsoft Word & Excel
- DaVinci Resolve

### Troubleshooting Method

Across these projects, I follow a repeatable troubleshooting structure:

1. Define the expected behavior
2. Capture the actual failure
3. Identify the layer involved: system, network, app, config, or permissions
4. Test one variable at a time
5. Apply the fix
6. Validate the result
7. Document what was learned

---

## Professional Direction

I am currently targeting roles in:

- IT Operations
- Technical Support
- Help Desk / Tier II Support
- Junior Systems Administration
- Network Support
- CRM Systems Support
- Salesforce / Litify Operations

I am also actively studying for the **Cisco CCNA** to strengthen my networking foundation.

---

## About This Portfolio

This portfolio is built from real troubleshooting work performed in my own environment. The focus is not just showing finished systems, but showing how I work through problems when systems break.

The goal is to show practical ability in:

- Diagnosing technical issues
- Working through incomplete information
- Documenting problems clearly
- Turning troubleshooting into repeatable knowledge
- Building confidence with real infrastructure

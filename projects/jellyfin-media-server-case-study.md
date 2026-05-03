# Home Media Server — Jellyfin Case Study

## Project Type
Self-hosted infrastructure / local network service / media server architecture

## Mission Objective
Build a private home media server using Jellyfin and design it in a way that supports local streaming, future expansion, and practical network access.

The goal was to create a useful self-hosted service while learning how server hardware, storage, client devices, and network access work together.

---

## Environment

| Component | Details |
|---|---|
| Server Role | Home media server |
| Media Platform | Jellyfin |
| Client Target | Apple TV ecosystem |
| Storage Planning | Local storage now, NAS expansion later |
| Network Context | Home LAN, future VPN access |

---

## Failure Index

1. Unsure whether a NAS was required immediately
2. Needed to understand server vs storage responsibilities
3. Needed a clean media folder structure
4. Needed local network access to Jellyfin
5. Needed future expansion without overspending early

---

## Phase 1 — Architecture Decision

### Question
Should the media server be built around a dedicated NAS, or could the existing PC/server handle the job?

### Initial Thought
A NAS might be required because media servers are often shown with dedicated network storage.

### Reality
A NAS is useful, but not required at the start. The existing server can run Jellyfin and host media locally.

### Decision
Start with the existing server, then expand to NAS storage later if the library grows.

### Lesson Learned
Do not buy extra infrastructure before defining the bottleneck. Build the simplest working version first, then expand based on real needs.

---

## Phase 2 — Jellyfin Role

### What Jellyfin Does
Jellyfin acts as the media server application. It scans media folders, organizes libraries, and serves content to client devices.

### Key Concept
Jellyfin is the service layer. It does not replace storage, networking, or permissions. It depends on those layers being configured properly.

### Mental Model

```text
Storage
  ↓
Media folders
  ↓
Jellyfin library scan
  ↓
Network access
  ↓
Client playback device
```

### Lesson Learned
When troubleshooting a media server, separate file storage problems from Jellyfin application problems and network access problems.

---

## Phase 3 — Media Folder Planning

### Goal
Create a folder layout that Jellyfin can understand and that stays clean as the library grows.

Example structure:

```bash
/media/movies
/media/tv
```

### Why This Matters
Clean folder structure reduces scanning confusion and makes future storage migration easier.

### Lesson Learned
Good folder organization is infrastructure. Messy storage creates future troubleshooting problems.

---

## Phase 4 — Local Network Access

### What Needed To Work
Devices on the home network needed to reach the Jellyfin server.

### Network Concerns
- Server IP stability
- Router behavior
- LAN access
- Potential future VPN access

### Solution Direction
Pair Jellyfin setup with home network improvements like DHCP reservation, so the server does not constantly change addresses.

### Lesson Learned
A service is easier to use and troubleshoot when the server has a predictable IP address.

---

## Phase 5 — Apple TV Playback Considerations

### Goal
Access Jellyfin from Apple TV or compatible client apps.

### Key Consideration
Client compatibility matters. A server can be working correctly while playback problems still happen on the client side.

### Lesson Learned
End-to-end service validation requires testing from the actual playback device, not just checking that the server dashboard loads.

---

## Before / After

| State | Description |
|---|---|
| Before | Media server idea was broad and mixed with NAS/storage uncertainty. |
| After | Clear architecture: use existing server first, organize media cleanly, validate local access, expand storage later. |

---

## Skills Demonstrated

- Self-hosted service planning
- Jellyfin architecture understanding
- LAN service access
- Storage planning
- Server vs NAS decision-making
- Client-device integration planning
- Cost-conscious infrastructure design
- Technical documentation

---

## Professional Relevance

This project maps to IT operations and technical support because it required:

- Designing a practical service
- Separating system layers
- Planning for users/devices
- Thinking through network access
- Avoiding unnecessary complexity
- Documenting setup decisions

# OpenClaw Gateway Setup & Troubleshooting

## Project Type
Homelab infrastructure / Linux service troubleshooting / local AI-control gateway experimentation

## Mission Objective
Set up and troubleshoot OpenClaw Gateway on an Ubuntu-based home server environment, integrate local control tooling, understand service behavior, and resolve access/configuration problems.

This project was valuable because it involved the kind of messy real-world troubleshooting that happens when documentation, security requirements, local services, network access, and config files all collide.

---

## Environment

| Component | Details |
|---|---|
| Host | Ubuntu Server 24.04 home server |
| User / Host Context | `brisco@bruisedworldwide` |
| Service | `openclaw-gateway.service` |
| Service Manager | `systemctl --user` |
| Config Path | `~/.openclaw/openclaw.json` |
| Gateway Version Observed | `v2026.4.15` |
| Gateway Port Observed | `18789` |
| Related Integration | Signal CLI daemon, Docker, CasaOS / local access |

---

## Failure Index

1. Gateway running but Control UI access blocked by origin rules
2. Control UI requiring device identity / secure context
3. Config JSON formatting issues
4. Confusion around allowed origins and LAN access
5. Reverse proxy header warning / trusted proxy warning
6. Docker socket permission issue
7. Signal CLI missing or not configured correctly
8. Need to inspect service logs and status without guessing

---

## Phase 1 — Service Status Check

### What I Did
Checked the OpenClaw gateway service:

```bash
systemctl --user status openclaw-gateway.service --no-pager -l
```

### What Happened
The service was active and running.

Observed behavior included:

- Loaded user service
- Active running state
- Main gateway process
- Resource usage
- Runtime logs

### Initial Assumption
If the service was running, the UI should work.

### Reality
A running service does not guarantee that the browser/UI access path is valid. The gateway process can be healthy while access is blocked by security, origin, or browser context requirements.

### Lesson Learned
Service status is only one layer. You still have to validate access path, authentication, origin, browser security context, and network exposure.

---

## Issue 1 — Control UI Origin Not Allowed

### Symptom
The browser returned an error similar to:

```text
origin not allowed
open the Control UI from the gateway host or allow it in gateway.controlUi.allowedOrigins
```

### What I Was Trying To Do
Access the Control UI from another device or through a local network address.

### Initial Assumption
The gateway was not running or not reachable.

### Root Cause
The gateway was enforcing allowed origin rules. The browser origin being used was not included in the config.

### Config Area Involved
Config path:

```bash
~/.openclaw/openclaw.json
```

Relevant area:

```json
"controlUi": {
  "allowedOrigins": [
    "http://10.0.0.1:18789",
    "https://bruisedworldwide.home.arpa"
  ]
}
```

### Troubleshooting Detail
At one point, the JSON structure had a missing comma between allowed origins:

```json
"allowedOrigins": [
  "http://10.0.0.1:18789"
  "https://bruisedworldwide.home.arpa"
]
```

That breaks JSON because array items require commas.

Corrected:

```json
"allowedOrigins": [
  "http://10.0.0.1:18789",
  "https://bruisedworldwide.home.arpa"
]
```

### Validation
After correcting JSON syntax and allowed origin values, restart/check the service:

```bash
systemctl --user restart openclaw-gateway.service
systemctl --user status openclaw-gateway.service --no-pager -l
```

### Lesson Learned
With web gateways, “service is running” does not mean “origin is allowed.” Browser origin rules can block access even when networking is correct.

---

## Issue 2 — Device Identity / Secure Context Requirement

### Symptom
The Control UI required device identity and complained about needing HTTPS or localhost secure context.

### What I Was Trying To Do
Open the UI from a LAN address.

### Root Cause
Modern browser security features treat localhost and HTTPS differently from plain HTTP over LAN. Some identity features require a secure context.

### Troubleshooting Thought Process
I separated the problem into:

- Is the gateway process running?
- Is the port listening?
- Is the browser origin allowed?
- Is the connection considered secure enough for device identity?
- Is there a reverse proxy or HTTPS path involved?

### Lesson Learned
Local network apps can still run into browser security rules. HTTP over LAN is not always treated the same as `localhost`.

---

## Issue 3 — Config JSON Troubleshooting

### What I Did
Inspected the config around the broken section:

```bash
nl -ba ~/.openclaw/openclaw.json | sed -n '20,40p'
```

### Why This Helped
Line-numbered output made it easier to identify syntax problems without guessing.

### Example Failure
A missing comma caused a JSON parse-style error:

```text
Expecting ',' delimiter
```

### Fix Method
Use line numbers to locate the broken area, then correct JSON syntax.

### Lesson Learned
For config files, line-numbered inspection is faster than eyeballing the whole file. JSON is unforgiving: one missing comma can break the service.

---

## Issue 4 — Binding and LAN Exposure

### Context
Gateway access involved settings like:

```json
"bind": "lan"
```

Or binding to non-loopback interfaces.

### Symptom
Warnings appeared when binding beyond localhost.

### Root Cause
Gateway software often treats LAN binding as higher-risk than localhost binding.

### Thought Process
The tradeoff was:

- `localhost` / loopback = safer, but less convenient from other devices
- LAN binding = accessible, but requires stronger origin/security controls

### Lesson Learned
When exposing local services to the LAN, tighten allowed origins and authentication. Convenience without boundaries creates risk.

---

## Issue 5 — Reverse Proxy / Trusted Proxy Warning

### Symptom
Security audit warning:

```text
gateway.trusted_proxies_missing
Reverse proxy headers are not trusted
```

### Meaning
The gateway detected proxy-style headers but did not have trusted proxy configuration fully defined.

### Troubleshooting Value
This helped identify that the access path might involve CasaOS, local proxy behavior, or forwarded headers.

### Lesson Learned
Reverse proxy warnings matter because incorrect proxy trust can affect identity, origin, client IP, and security logic.

---

## Issue 6 — Docker Socket Permission Problem

### Symptom
Error similar to:

```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
```

### Initial Assumption
Docker may not be running.

### Root Cause
The user/service did not have permission to access the Docker socket.

### Lesson Learned
Docker permission problems are access-control problems, not always Docker runtime problems. The socket path is protected because Docker access can effectively grant powerful system control.

---

## Issue 7 — Signal CLI Integration

### Symptom
OpenClaw channel setup reported:

```text
signal-cli not found. Install it, then rerun this step or set channels.signal.cliPath.
```

### Fix Path
Signal CLI was installed and made available at:

```bash
/usr/local/bin/signal-cli
```

Version observed:

```text
0.14.2
```

A daemon pattern was used:

```bash
/usr/local/bin/signal-cli -a <account> daemon --http 127.0.0.1:8080 --no-receive-stdout
```

### Additional Issue
Signal CLI registration and account handling required understanding multi-account mode and captcha flow.

### Lesson Learned
Integrations fail in layers: first the binary path, then account state, then daemon behavior, then the calling application’s config.

---

## Final System Understanding

By the end of troubleshooting, the OpenClaw setup was understood as a layered system:

```text
Browser / Control UI
        ↓
Origin + Secure Context Rules
        ↓
OpenClaw Gateway
        ↓
Config JSON
        ↓
Systemd User Service
        ↓
Local Integrations
        ↓
Signal CLI / Docker / CasaOS / Network
```

When something failed, the question became: which layer is failing?

---

## Skills Demonstrated

- Linux service troubleshooting
- `systemctl --user` usage
- Config file debugging
- JSON syntax validation
- Browser security context awareness
- Origin restriction troubleshooting
- Reverse proxy warning interpretation
- Docker permission awareness
- Signal CLI integration troubleshooting
- Layered systems thinking
- Technical documentation

---

## Professional Relevance

This project maps well to IT support, systems support, junior sysadmin, and technical operations work because it required:

- Reading logs
- Checking service state
- Debugging config syntax
- Understanding local network access
- Separating app issues from OS/service issues
- Interpreting permission problems
- Documenting root cause and fix paths

The biggest professional lesson: a running service is only one checkpoint. Real troubleshooting requires validating the service, config, network path, browser/client requirements, and integration dependencies.

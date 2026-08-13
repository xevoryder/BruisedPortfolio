# NetWatch Command — Network Operations Dashboard

## Objective

Build and operate a small, self-hosted network monitoring application that checks real homelab devices, services, and Docker workloads while producing safe, reviewable evidence of system health.

The project was designed as more than a dashboard mockup. It runs as a live Linux service, collects operational results, preserves history in SQLite, exposes a local web dashboard and JSON API, and includes automated tests.

**Public repository:** [xevoryder/netwatch-command](https://github.com/xevoryder/netwatch-command)

---

## Environment

| Component | Implementation |
|---|---|
| Host | Ubuntu/Linux home server |
| Application | Python standard-library service |
| Service management | user `systemd` |
| Device checks | ICMP availability and latency |
| Service checks | TCP socket connectivity |
| Container checks | Read-only Docker runtime and health inventory |
| History | SQLite |
| Interface | Local web dashboard and JSON API |
| Remote access | Private WireGuard path protected by UFW |
| Validation | Python unit tests and live HTTP checks |

Real target addresses and hostnames remain in a local, ignored configuration file. The public repository includes only a sanitized example configuration.

---

## Architecture

```text
Sanitized TOML target configuration
        |
        v
Device ping checks + TCP service checks
        |
        +----> Read-only Docker inventory
        |
        v
SQLite result history
        |
        +----> JSON status API
        |
        +----> Local web dashboard
```

NetWatch separates three kinds of operational evidence:

1. **Device reachability** — whether a host responds to ICMP and with what latency.
2. **Service reachability** — whether a configured TCP port accepts a connection.
3. **Container state** — whether a Docker workload is running, stopped, restarting, starting, healthy, unhealthy, degraded, or unknown.

This prevents a single green indicator from hiding failures at another layer.

---

## Implementation

### Configuration-driven monitoring

Targets are declared in TOML rather than hard-coded into the application. The tracked example uses documentation-safe addresses, while the live deployment file is excluded from Git.

For containerized services, NetWatch checks the host-published port rather than assuming the container's internal port is reachable from the host.

### Historical evidence

Each monitoring run stores results in SQLite. This turns transient status checks into reviewable evidence that can support incident analysis and future trend reporting.

### Safe Docker awareness

Docker inventory uses constrained, read-only commands and requests only operational fields:

- runtime state
- configured health-check state
- restart count
- start time
- host-published ports

It does not collect container environment variables, labels, mount details, or file contents. Docker failures are converted into an `UNKNOWN` result without preventing device and TCP checks from running.

### Dashboard and API

The application exposes:

- a browser dashboard for current status
- a JSON endpoint for machine-readable status
- sanitized request logging for access-path troubleshooting

The live service is managed through user `systemd` so monitoring survives terminal sessions and can be inspected with standard Linux service tooling.

---

## Troubleshooting Incident: WireGuard Access Blocked by UFW

### Expected behavior

A connected WireGuard client should reach the dashboard through the server's private VPN address.

### Actual behavior

The dashboard returned HTTP `200` locally but timed out from a phone connected through WireGuard.

### Diagnostic process

1. Verified that the NetWatch service was active.
2. Confirmed the application listened on the intended host interfaces.
3. Confirmed the dashboard and API returned HTTP `200` locally.
4. Verified recent WireGuard handshakes and traffic movement.
5. Observed that no failed remote request reached the application access log.
6. Correlated the client attempt with a UFW block on the WireGuard interface and dashboard TCP port.

The missing application-log entry was useful negative evidence: the request failed before application handling. The firewall log then located the drop at the host INPUT boundary.

### Root cause

The application was listening correctly, and the VPN tunnel was active, but UFW had no INPUT allowance for WireGuard clients to reach the NetWatch port.

### Correction

Applied a narrow firewall allowance constrained to the WireGuard interface, private VPN source range, server VPN destination, TCP protocol, and dashboard port. A broad allow rule across all interfaces was deliberately avoided.

### Validation

The same client workflow was repeated after the correction:

```text
Phone browser
→ WireGuard tunnel
→ server VPN interface
→ narrow UFW allowance
→ NetWatch listener
→ status API HTTP 200
→ dashboard rendered
```

This validated the real user path rather than stopping at service state or VPN handshake status.

---

## Current Validation Baseline

At the documented V1 milestone:

- the user `systemd` service was active
- the dashboard returned HTTP `200`
- the status API returned HTTP `200`
- all **14 unit tests** passed
- the public Git history was rebuilt as a sanitized root commit
- the operational target file remained local and ignored
- public-tree scans found no reviewed deployment-specific or credential-shaped values

The public repository is intentionally a single sanitized baseline rather than a copy of private development history that once contained local deployment details.

---

## What This Demonstrates

- Python application development tied to a real operational need
- Linux service management with `systemd`
- ICMP, TCP, HTTP, and container-health distinctions
- Configuration-driven monitoring
- SQLite persistence
- Read-only Docker inspection with failure isolation
- Layered troubleshooting across application, socket, VPN, and firewall boundaries
- Least-privilege firewall correction
- Automated testing and functional endpoint validation
- Git history and configuration sanitization before public release
- Clear technical documentation without exposing private infrastructure

---

## Professional Relevance

This project maps directly to network support, systems support, technical operations, and junior infrastructure roles. It demonstrates the ability to:

- build tooling around an operational problem
- distinguish host, network, service, container, and application states
- collect evidence before changing configuration
- diagnose a remote-access failure layer by layer
- apply the smallest corrective change
- verify both automated checks and the real client experience
- publish useful proof of work without publishing deployment secrets

---

## Next Improvement

Add application-level HTTP response probes that verify an expected status code and response behavior—not merely that a TCP port accepts connections. This will establish a second milestone: moving from transport availability to application availability.

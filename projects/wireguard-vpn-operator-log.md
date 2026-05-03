# WireGuard VPN Server Deployment & Debugging

## Project Type
Homelab infrastructure / networking / Linux troubleshooting

## Mission Objective
Deploy a functional WireGuard VPN server on Ubuntu Server 24.04 with mobile iPhone access and full traffic routing through the VPN tunnel.

The goal was not only to “make a VPN connect,” but to confirm that the tunnel actually routed traffic correctly and could provide practical remote access into a home network environment.

---

## Environment

| Component | Details |
|---|---|
| Server OS | Ubuntu Server 24.04 |
| VPN Software | WireGuard |
| Mobile Peer | iPhone |
| Router | ASUS ROG Rapture GT-AC5300 |
| VPN Network | `10.0.0.0/24` |
| Server VPN Address | `10.0.0.1/24` |
| Main Use Case | Secure remote access into home infrastructure |

---

## Failure Index

1. Permission denied when working inside `/etc/wireguard/`
2. Confusion around QR code generation for mobile peer setup
3. Mobile peer config existed but did not behave correctly
4. VPN showed as connected but traffic did not route as expected
5. Needed to understand the difference between tunnel connection and actual network routing

---

## Phase 1 — Initial WireGuard Setup

### What I Was Trying To Do
Install WireGuard and create the baseline VPN configuration.

### Commands / Actions
```bash
sudo apt update
sudo apt install wireguard
```

Key generation pattern:
```bash
wg genkey | tee privatekey | wg pubkey > publickey
```

### Expected Result
WireGuard installed, keys generated, and server config ready for peer creation.

### Actual Result
The basic install path made sense, but the problems started once configuration files and permissions became involved.

### Lesson
Installation is not the same as deployment. The real work begins when the service has to be configured, secured, and validated.

---

## Issue 1 — Permission Denied Lockout

### What I Did
Tried to inspect WireGuard configuration files:

```bash
ls -l /etc/wireguard/
```

### What Happened
The terminal returned a permission denied error.

### Initial Assumption
My first thought was that something might be wrong with the directory, the installation, or the config path.

### Root Cause
This was normal Linux permissions behavior. The `/etc/wireguard/` directory contains sensitive VPN keys and configuration data, so it requires elevated privileges.

### Fix
Use `sudo` when interacting with WireGuard config files:

```bash
sudo ls -l /etc/wireguard/
sudo nano /etc/wireguard/wg0.conf
```

### Validation
Once commands were run with proper privileges, the directory could be inspected and configuration work could continue.

### Lesson Learned
Always verify privilege level before assuming a system is broken. A permissions issue can look like a configuration issue if the operator skips the access-control layer.

---

## Issue 2 — QR Code Generation Problems

### What I Was Trying To Do
Generate a QR code so the iPhone WireGuard app could import the peer configuration.

### Expected Result
A scannable QR code should appear in the terminal.

### Actual Result
The QR process did not work cleanly at first.

### Initial Assumption
The mobile config might have been formatted wrong.

### Root Cause
The QR code utility was missing or not being called correctly.

### Fix
Install `qrencode`:

```bash
sudo apt install qrencode
```

Generate QR from the mobile config:

```bash
sudo cat iphone.conf | qrencode -t ansiutf8
```

### Validation
Once the QR code displayed correctly, the iPhone could scan/import the config.

### Lesson Learned
A missing dependency can look like a bad config. Before rewriting configuration files, confirm that the tooling needed to process them is installed.

---

## Issue 3 — Mobile Peer Config Problems

### What I Was Trying To Do
Create a usable iPhone peer configuration.

### Expected Result
The iPhone should import the config, activate the tunnel, and connect to the server.

### Actual Result
The config could exist as a file but still fail because the values inside it were incomplete, malformed, or mismatched.

### Troubleshooting Thought Process
I separated the problem into two sides:

- **Server side:** Is WireGuard listening and does the server know about the peer?
- **Client side:** Does the iPhone config contain correct private key, address, endpoint, server public key, and allowed IPs?

### Example Clean Client Config Structure
```ini
[Interface]
PrivateKey = <client_private_key>
Address = 10.0.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = <server_public_key>
Endpoint = <public_ip_or_dns>:51820
AllowedIPs = 0.0.0.0/0
```

### Fix
Instead of endlessly patching a questionable config, the cleaner move was to rebuild the peer config manually and verify each field.

### Validation
The iPhone could import the peer profile and attempt connection.

### Lesson Learned
When a config has been changed multiple times and still behaves wrong, rebuilding from a clean known-good structure is often faster than patching line-by-line.

---

## Issue 4 — VPN Connected But Traffic Not Routing

### What I Was Trying To Do
Confirm that the iPhone was not only connected to WireGuard, but actually routing traffic through the VPN.

### Expected Result
Traffic should pass through the VPN tunnel.

### Actual Result
The VPN could show connected while the device still behaved like it was using its original network path.

### Root Cause
Connection and routing are different layers.

A VPN can establish a tunnel but still fail to route the traffic the way the operator expects.

### Fix Areas
Client allowed IPs:

```ini
AllowedIPs = 0.0.0.0/0
```

Server IP forwarding:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

### Validation
Validation required more than seeing “connected” in the app.

Tests included:

```bash
ping 10.0.0.1
```

And checking whether traffic/IP behavior changed from the client side.

### Lesson Learned
Do not stop at “the tunnel connects.” Validate the traffic path. In networking, status indicators can be misleading unless they are paired with functional tests.

---

## Before / After

| State | Description |
|---|---|
| Before | WireGuard pieces existed, but permissions, QR generation, config correctness, and routing behavior were not fully aligned. |
| After | Mobile peer onboarding worked, VPN connection was functional, and routing behavior was understood and validated. |

---

## Troubleshooting Method Used

1. Confirm the expected behavior.
2. Capture the actual failure.
3. Identify which layer might be responsible: permissions, dependency, config, service, routing, or client behavior.
4. Test one change at a time.
5. Rebuild messy configs when patching became inefficient.
6. Validate with real connectivity/routing tests.
7. Document the lesson.

---

## Skills Demonstrated

- Ubuntu Server administration
- Linux file permission troubleshooting
- WireGuard configuration
- Mobile VPN peer setup
- QR code provisioning
- VPN routing concepts
- Client/server config validation
- Network testing
- Technical documentation

---

## Professional Relevance

This project maps directly to IT support, network support, junior systems administration, and security support because it required:

- Working with Linux config files
- Troubleshooting access problems
- Understanding VPN behavior
- Testing connectivity
- Documenting steps clearly
- Separating symptoms from root cause

# WireGuard VPN Deployment & Debugging

## Objective
Deploy a secure VPN using WireGuard on Ubuntu Server 24.04 with mobile (iPhone) access.

## Problems Encountered
- Permission denied errors on `/etc/wireguard/`
- QR code generation (`qrencode`) not working
- Invalid or malformed peer configuration
- VPN connected but traffic not routing through tunnel

## Troubleshooting Process
- Verified permissions using `ls -l`
- Escalated privileges using `sudo`
- Installed missing dependencies (`qrencode`)
- Inspected and rebuilt configuration files manually
- Adjusted `AllowedIPs` and interface settings
- Tested connectivity vs actual routing behavior

## Solutions
- Fixed file permissions and ownership
- Successfully generated QR code for mobile onboarding
- Rebuilt peer configuration cleanly
- Corrected routing to force VPN traffic through tunnel

## Result
- Functional VPN with secure remote access
- Stable routing through private network

## Time to Resolution
- Setup: 1–2 hours
- Debugging: 2–4 hours

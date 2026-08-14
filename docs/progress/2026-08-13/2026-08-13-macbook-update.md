# MacBook Pro 15,1 — Ubuntu Server Infrastructure Update

**Date:** 13 August 2026  
**Status:** Operational

## Migration from macOS

The MacBook was moved away from macOS after persistent host firewall/content-filter behaviour complicated self-hosted infrastructure services.

Ubuntu Server is now the primary operating system and the machine is treated as an independent infrastructure/support node rather than a Proxmox cluster member.

## Wi-Fi Recovery

The Broadcom BCM4364 Wi-Fi problem carried over from the previous entry was resolved using offline packages prepared on another Linux machine and transferred over USB.

Confirmed outcomes:

- Required firmware/packages installed
- NetworkManager available
- `wpa_supplicant` available
- Wireless interface detected and operational
- Normal local-network latency restored after initial troubleshooting

This closes the Wi-Fi loop left open on 12 August.

## SSH

SSH administration to the server was confirmed working.

This replaced the earlier macOS-specific SSH/firewall troubleshooting with a simpler Linux administration path.

## WireGuard

The infrastructure server was added as a peer to the existing Proxmox management WireGuard network.

The obsolete earlier MacBook peer/configuration was removed and the new Linux WireGuard configuration was enabled at boot.

## DNS

Unbound was installed for lab-oriented DNS service.

Initial SERVFAIL behaviour was corrected and successful recursive/forwarded resolution was confirmed.

The design intention is:

- Lab/security environments can use the infrastructure server for DNS where useful.
- Other devices use secure upstream DNS directly.
- The DNS design should remain compatible with future VLAN segmentation.

## Secure Upstream DNS

Quad9 was selected as the secure upstream DNS provider for non-lab traffic.

This configuration was applied to the infrastructure server and also adopted elsewhere in the platform where appropriate.

## Current State

```text
MacBook role:       Ubuntu Server infrastructure node
Wi-Fi:              Working
SSH:                Working
WireGuard:          Working
Unbound:            Working
Secure upstream:    Configured
```

## Next Actions

- Add monitoring when useful.
- Keep DNS compatible with future VLANs.
- Evaluate Wake-on-LAN / local power-control workflows.
- Add other infrastructure services only when they have a clear purpose.

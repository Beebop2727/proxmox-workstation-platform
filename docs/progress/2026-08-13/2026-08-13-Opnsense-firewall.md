# OPNsense Firewall Deployment & Network Integration

**Date:** 13 August 2026  
**Status:** 🟡 In Progress

## Overview

The homelab was extended with a dedicated OPNsense firewall/router VM running on Proxmox VE.

The session covered:

- Creating and installing the OPNsense VM
- Correcting installation/boot configuration issues
- Configuring separate WAN and LAN interfaces
- Enabling LAN DHCP
- Restoring reliable access to the web GUI
- Establishing the foundation for a dedicated WireGuard management tunnel
- Verifying the Proxmox-to-OPNsense virtual network path

## VM 104

```text
VM ID:          104
Operating System: OPNsense
Disk:           16 GB
vCPU:           2
Memory:         4 GB at initial deployment
Machine:        q35
```

Network interfaces:

```text
vtnet0 -> WAN -> Proxmox private network
vtnet1 -> LAN -> isolated firewall-side network
```

## Addressing

```text
WAN:  10.10.10.144/24
LAN:  10.10.99.1/24
```

LAN DHCP was configured for clients on the firewall-side network.

## Web Management

HTTPS management was confirmed from the Proxmox-side network.

One important firewall lesson was that WAN private-network blocking can interfere with a lab design where the OPNsense WAN itself is connected to an RFC1918 network.

A controlled management rule was retained rather than broadly opening the interface.

## WireGuard Foundation

A separate WireGuard instance was created for firewall management:

```text
OPNsense tunnel address: 10.255.99.1/24
Listener:                UDP 51821
```

The peer configuration was created, but the end-to-end management tunnel was not yet complete at the end of this entry.

## State at End of Session

```text
OPNsense installed:      Yes
WAN/LAN configured:      Yes
LAN DHCP:                Yes
Web GUI:                 Working
WireGuard instance:      Configured
WireGuard management:    Not yet complete
```

## Next Action

Complete packet forwarding and firewall rules for the trusted WireGuard management peer.

This loop was completed on 14 August 2026; see `../2026-08-14/README.md`.

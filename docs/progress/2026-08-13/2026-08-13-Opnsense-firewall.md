# OPNsense Firewall Deployment & Network Integration

**Date:** 13 August 2026  
**Status:** 🟡 In Progress

---

## Overview

Today the homelab was extended with a dedicated **OPNsense firewall/router VM** running on Proxmox VE.

The work covered:

- Creating and installing the OPNsense VM.
- Troubleshooting the OPNsense installation disk.
- Correcting the VM boot configuration.
- Configuring separate WAN and LAN interfaces.
- Configuring the OPNsense WAN and LAN addresses.
- Enabling LAN DHCP.
- Accessing the OPNsense web GUI.
- Setting up the foundations for a secondary WireGuard management tunnel.
- Troubleshooting connectivity between the ThinkPad, Proxmox and OPNsense.
- Verifying the Proxmox virtual networking and OPNsense WAN interface.
- Shutting the OPNsense VM down cleanly at the end of the session.

---

# 1. OPNsense VM

OPNsense was deployed as **VM 104** on the Proxmox server.

### VM

```text
VM ID:       104
Operating System: OPNsense
Disk:        16 GB
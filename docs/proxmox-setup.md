# Proxmox setup

Documentation of the current Proxmox VE installation and the main host configuration decisions.

## BIOS configuration

| Setting | Value | Notes |
|---|---|---|
| SVM Mode | Enabled | AMD virtualisation |
| IOMMU | Enabled | Required for PCIe passthrough |
| Above 4G Decoding | Enabled | Required for GPU passthrough |
| UEFI boot | Enabled | Current host/guest design |

## Installation

**Target drive:** 500 GB NVMe SSD

The Proxmox host is intended to remain minimal. Desktop workloads, browsing, development, and gaming belong inside guests or the separate bare-metal Windows environment.

## Storage

| Storage | Purpose |
|---|---|
| 500 GB NVMe | Proxmox VE / core host storage |
| 4 TB NVMe | Primary VM storage |
| 2 × 2 TB HDD | Future backup / bulk storage |

The HDD backup design remains a planned follow-up item.

## Networking

The current location does not provide a convenient wired router connection, so the host uses the Intel AX200 as an upstream Wi-Fi client.

A Wi-Fi station interface cannot normally be attached to a conventional Linux bridge in the same manner as Ethernet. The current design therefore routes and NATs the private VM network through the host.

```text
Upstream Wi-Fi
      |
   Proxmox
      |
  routing/NAT
      |
    vmbr0
 10.10.10.0/24
      |
     VMs
```

Current private bridge:

```text
vmbr0: 10.10.10.1/24
```

OPNsense is connected to the private Proxmox network and a second isolated LAN-side bridge for future firewall-controlled lab networking.

A wired upstream link is preferred in the long term.

## Management

Management paths include:

- Proxmox HTTPS / SSH from trusted administration devices
- A dedicated Proxmox WireGuard management network
- A separate OPNsense WireGuard management network

These tunnels are intentionally independent.

## Recovery principle

When changing routing, firewalling, or VPN configuration:

1. Keep an alternate management path available.
2. Verify packet arrival with `tcpdump`.
3. Verify routes and firewall counters before changing keys or tunnel addresses.
4. Change one layer at a time.

# Proxmox setup

Documentation of the Proxmox VE installation and configuration process.

---

## BIOS configuration

Before installing Proxmox, the following settings were enabled in the MSI MPG X570 Gaming Pro Carbon WiFi BIOS:

| Setting | Value | Notes |
|---|---|---|
| SVM Mode | Enabled | AMD virtualisation support |
| IOMMU | Enabled | Required for PCIe passthrough |
| Above 4G Decoding | Enabled | Required for GPU passthrough |
| Resizable BAR | Disabled | Can cause passthrough issues |

---

## Installation

**Target drive:** 500 GB NVMe SSD

**Proxmox VE version:**

**Installation date:**

### Steps

1. Downloaded Proxmox VE ISO from [proxmox.com](https://www.proxmox.com)
2. Flashed to USB using Balena Etcher / Rufus
3. Booted from USB and ran installer
4. Set management interface to motherboard NIC
5. Configured static IP for web UI access

---

## Storage configuration

| Pool | Device | Purpose |
|---|---|---|
| local | 500 GB NVMe | Proxmox OS, ISO storage, VM templates |
| vm-storage | 4 TB NVMe | Ubuntu VM disk, Windows VM disk, game library |
| bulk | 2× 2 TB HDD | Backups, media, project archives |

### Steps taken

<!-- Document the storage setup process here as you go -->

---

## Network configuration

| Bridge | Interface | Purpose |
|---|---|---|
| vmbr0 | Motherboard NIC | VM network access |

<!-- Add further network notes here -->

---

## Web UI access

Proxmox web UI is accessible at `https://<host-ip>:8006` from the ThinkPad and MatePad Pro.

---

## Notes and observations

<!-- Add anything useful that came up during setup -->

# Proxmox Workstation Platform

> A Proxmox-based workstation and homelab platform separating Linux productivity, Windows gaming, security testing, firewalling, and supporting infrastructure across a small set of physical and virtual systems.

**Current phase:** V2 Under Development
**Last updated:** 23 August 2026

## Overview

This project documents the design and implementation of a virtualized workstation platform built around Proxmox VE.

The original goal was to separate day-to-day Linux productivity from Windows gaming while retaining a single physical workstation. The project has since expanded into a small homelab with dedicated security environments, an OPNsense firewall VM, a separate Ubuntu Server infrastructure host, and multiple WireGuard management paths.

The platform currently includes:

- An Ubuntu workstation VM with AMD GPU passthrough
- A Windows 11 VM with NVIDIA GPU passthrough and Looking Glass
- A separate bare-metal Windows 11 installation for native / anti-cheat-sensitive games
- Reusable and disposable Parrot OS security VMs
- An OPNsense firewall/router VM
- A dedicated OPNsense WireGuard management tunnel
- A separate Ubuntu Server infrastructure host
- Routed/NAT virtual networking over the current Wi-Fi-constrained uplink
- Tested VM backup and restoration workflows
- Grafana and Prometheus infrastructure monitoring
- An isolated OPNsense-backed cybersecurity lab network
- Experimental Dockur Windows / WinApps integration for Windows-only desktop applications

## Current status

| Area | Status |
|---|---|
| Proxmox VE host | Operational |
| 4 TB NVMe VM storage | Operational |
| Ubuntu workstation VM | Operational |
| Windows 11 gaming VM | Operational and play-tested |
| Bare-metal Windows 11 | Operational |
| Parrot OS security environment | Operational |
| AMD Radeon Pro WX 3100 passthrough | Operational |
| NVIDIA GeForce RTX 4070 passthrough | Operational |
| Looking Glass B7 | Operational |
| Private routed VM network | Operational |
| Proxmox WireGuard management | Operational |
| Ubuntu Server infrastructure host | Operational |
| OPNsense firewall VM | Operational |
| OPNsense WireGuard management | Operational |
| VM backup / restore validation | Complete |
| Public documentation cleanup | In progress |
| Grafana / Prometheus monitoring | Operational |
| Uptime Kuma monitoring | In development |
| Isolated OPNsense lab network | Operational |
| Dockur Windows compatibility environment | Proof of concept operational |
| WinApps / RemoteApp integration | In development |
| VLAN segmentation | Planned |
| Wired Proxmox uplink | Planned |

## Documentation

- [Project roadmap](ROADMAP.md)
- [Progress log index](docs/progress/README.md)
- [Latest progress — 21 August 2026: Dockur Windows + WinApps proof of concept](docs/progress/2026-08-21/2026-08-21-dockur-winapps-progress.md)
- [Proxmox setup notes](docs/proxmox-setup.md)
- [GPU passthrough notes](docs/gpu-passthrough-notes.md)
- [Lessons learned](docs/lessons-learned.md)
- [Decision records and research-brain design](docs/decisions/README.md)
- [Windows environment decision](docs/decisions/2026-08-05-windows-environments.md)
- [Distributed infrastructure decision](docs/decisions/2026-08-10-distributed-infrastructure.md)
- [Original V1 architecture diagram](architecture/proxmox_workstation_v1_architecture.svg)

## Architecture

The current platform is moving from a single-host workstation into a small distributed homelab.

```text
                         Upstream LAN / Wi-Fi
                                  |
              +-------------------+-------------------+
              |                                       |
         Proxmox VE                           Ubuntu Server
       virtualization                      infrastructure host
              |                                       |
       +------+------+------+                   DNS / labs
       |      |      |      |                  WireGuard peer
    Ubuntu  Windows Parrot OPNsense
      VM      VM     VMs   firewall
       |       |
       +-- Looking Glass
              |
          Management
              |
           ThinkPad

OPNsense dedicated management tunnel:
Trusted client <---- WireGuard ----> OPNsense

Separate Proxmox management WireGuard remains independent.
```

The current Wi-Fi-based design is deliberately temporary. The VM network is routed and NATed through the host because a normal Wi-Fi client interface cannot generally be used as a conventional Layer 2 bridge like Ethernet.

## Hardware

| Component | Specification |
|---|---|
| CPU | AMD Ryzen 9 3900X |
| Motherboard | MSI MPG X570 Gaming Pro Carbon WiFi |
| RAM | 64 GB DDR4 |
| Windows GPU | NVIDIA GeForce RTX 4070 |
| Ubuntu GPU | AMD Radeon Pro WX 3100, 4 GB |
| Wireless adapter | Intel Wi-Fi 6 AX200 |
| Hypervisor storage | 500 GB NVMe SSD |
| Primary VM storage | 4 TB NVMe SSD |
| Bulk storage | 2 × 2 TB HDD |
| PSU | EVGA 750 GQ — 750 W, 80 Plus Gold |
| Case | NZXT H510 Elite |
| Display | Huawei MateView GT 34-inch ultrawide |

The physical card layout differs from the original plan because of card size and cooler clearance:

```text
Top PCIe slot:     AMD Radeon Pro WX 3100
Lower PCIe slot:   NVIDIA GeForce RTX 4070
```

## Main virtual machines

### VM 100 — Windows 11

The Windows VM remains available for compatible games, Windows applications, testing, and maintenance tasks.

- OVMF / UEFI
- TPM 2.0
- RTX 4070 passthrough
- Looking Glass B7 host
- IVSHMEM / KVMFR integration
- Successfully play-tested

A separate bare-metal Windows installation is used where native execution or anti-cheat compatibility is preferable.

### VM 101 — Ubuntu Workstation

The Ubuntu VM is the primary virtual Linux desktop and integration environment.

- OVMF / UEFI
- AMD Radeon Pro WX 3100 passthrough
- QEMU Guest Agent
- Development and administration environment
- Looking Glass client
- Docker with nested KVM access
- Experimental Dockur Windows 11 LTSC environment
- WinApps / RemoteApp integration testing

### VM 102 / 103 — Parrot OS

The Parrot environment separates security tooling and disposable lab work from the main workstation.

- Reusable base VM
- Disposable clone
- No dedicated GPU required
- Intended for controlled cybersecurity labs and testing

### VM 104 — OPNsense

OPNsense provides the firewall foundation for later network segmentation.

Current lab-side addressing:

```text
WAN:        10.10.10.144/24
LAN:        10.10.99.1/24
WireGuard:  10.255.99.1/24
```

A trusted management peer uses the dedicated `10.255.99.0/24` WireGuard network. UDP forwarding from the current Proxmox uplink is restored automatically at host boot using a small systemd service.

The OPNsense management tunnel is intentionally separate from the existing Proxmox WireGuard management network.

## Infrastructure host

An older Intel MacBook was repurposed from macOS into Ubuntu Server after macOS network filtering caused repeated problems with self-hosted infrastructure services.

Current role:

- Independent infrastructure/support node
- SSH administration
- WireGuard connectivity
- Unbound DNS for lab use
- Secure upstream DNS configuration
- Future monitoring, wake, and supporting services

It is intentionally not a Proxmox cluster member.

## Networking

Two management paths currently coexist:

1. **Proxmox management WireGuard** — retained as an independent management route to the hypervisor and existing lab network.
2. **OPNsense management WireGuard** — dedicated tunnel into the firewall itself.

This separation means changes to the OPNsense tunnel do not require modifying the already-working Proxmox WireGuard configuration.

Current limitations:

- Proxmox currently uses Wi-Fi as its upstream connection.
- Wi-Fi testing has shown intermittent packet loss and latency spikes between some local clients.
- A future wired uplink is strongly preferred before advanced VLAN segmentation.

## Repository structure

```text
proxmox-workstation-platform/
├── LICENSE
├── README.md
├── ROADMAP.md
├── architecture/
│   └── proxmox_workstation_v1_architecture.svg
├── docs/
│   ├── decisions/
│   │   ├── README.md
│   │   ├── 2026-08-05-windows-environments.md
│   │   ├── 2026-08-10-distributed-infrastructure.md
│   │   ├── 2026-08-17-ram-upgrade-decision.md
│   │   └── 2026-08-25-hyprland-decision.md
│   ├── gpu-passthrough-notes.md
│   ├── lessons-learned.md
│   ├── proxmox-setup.md
│   └── progress/
│       ├── README.md
│       ├── 2026-08-03/
│       │   └── 2026-08-03-document-proxmox-progress-1.md
│       ├── 2026-08-10/
│       │   └── 2026-08-10-proxmox-continuation.md
│       ├── 2026-08-11/
│       │   └── 2026-08-11-macbook-infrastructure.md
│       ├── 2026-08-12/
│       │   └── 2026-08-12-macbook-server.md
│       ├── 2026-08-13/
│       │   ├── 2026-08-13-Opnsense-firewall.md
│       │   └── 2026-08-13-macbook-update.md
│       └── 2026-08-14/
│           └── 2026-08-14-OPNsense-firewall-cleanup.md
└── scripts/
    └── scripts-readme.md
```

## Progress history

The dated progress logs preserve the project as it actually developed rather than rewriting older entries to match later decisions.

- [3 August — core Proxmox workstation progress](docs/progress/2026-08-03/2026-08-03-document-proxmox-progress-1.md)
- [10 August — Proxmox continuation and distributed infrastructure direction](docs/progress/2026-08-10/2026-08-10-proxmox-continuation.md)
- [11 August — MacBook infrastructure / macOS filtering work](docs/progress/2026-08-11/2026-08-11-macbook-infrastructure.md)
- [12 August — MacBook migration to Ubuntu Server](docs/progress/2026-08-12/2026-08-12-macbook-server.md)
- [13 August — MacBook Ubuntu Server update](docs/progress/2026-08-13/2026-08-13-macbook-update.md)
- [13 August — OPNsense deployment](docs/progress/2026-08-13/2026-08-13-Opnsense-firewall.md)
- [14 August — OPNsense WireGuard and firewall cleanup](docs/progress/2026-08-14/2026-08-14-OPNsense-firewall-cleanup.md)
- [15 August — Grafana and Prometheus monitoring foundation](docs/progress/2026-08-15/2026-08-15-monitoring-progress.md)
- [18 August — 64 GB RAM upgrade and testing](docs/progress/2026-08-18/2026-08-18-ram-successfully-installed-and-tested.md)
- [19 August — OPNsense lab configuration and Uptime Kuma deployment](docs/progress/2026-08-19/2026-08-19-OPNsense-and-lab-configuration.md)
- [21 August — Dockur Windows and WinApps proof of concept](docs/progress/2026-08-21/2026-08-21-dockur-winapps-progress.md)

## Learning outcomes

This project demonstrates practical experience with:

- Bare-metal virtualization
- Linux administration
- PCIe / GPU passthrough
- IOMMU and UEFI configuration
- Virtual storage management
- VM templates and disposable environments
- Routed and NAT-based VM networking
- Wi-Fi-constrained infrastructure design
- WireGuard
- OPNsense firewalling
- Packet capture and multi-hop network troubleshooting
- Linux connection tracking and NAT
- Backup and restoration
- Looking Glass shared-memory display
- Distributed infrastructure design
- Technical troubleshooting and documentation

## Security and publication

This is a public repository. Configuration examples are sanitized before publication.

The repository must not contain:

- Passwords
- Private keys
- WireGuard private keys
- VPN credentials
- API tokens
- Cookies
- Personally identifying paths or data
- Unnecessary public IP addresses

Private RFC1918 addresses may be documented where they help explain the lab architecture, but they are not credentials.

## Current priorities

See the [roadmap](ROADMAP.md) for the full list. The main near-term items are:

- Perform final cold-boot verification of the OPNsense management path
- Reserve or statically assign infrastructure addresses relied on by persistent forwarding
- Review and tune VM memory allocation against the host's 64 GB RAM capacity
- Move the Proxmox uplink to Ethernet when practical
- Introduce VLAN segmentation when the physical network is ready
- Continue the backup and monitoring work
- Update the architecture diagram as the distributed design stabilizes

## Disclaimer

This is a personal learning and workstation project built on consumer hardware. It is not intended to provide enterprise high availability or production service guarantees.

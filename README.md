# Proxmox Workstation Platform

> A Proxmox-based workstation platform that separates Linux productivity, Windows gaming, and supporting virtual workloads on one physical desktop.

**Current phase:** V1 operational, V1.5 integration and refinement  
**Last updated:** 3 August 2026

## Overview

This project documents the design and implementation of a virtualized workstation platform built using Proxmox VE.

The primary goal is to separate day-to-day productivity workloads from gaming workloads while maintaining a single physical desktop. Rather than relying exclusively on dual booting, the platform uses virtual machines with dedicated hardware resources assigned to each environment.

The system currently centres on:

- An Ubuntu workstation VM with AMD GPU passthrough
- A Windows 11 gaming VM with NVIDIA GPU passthrough
- A Parrot OS security environment with a reusable base VM and disposable clone
- Looking Glass for low-latency Windows display inside Ubuntu
- Routed virtual networking over a Wi-Fi-constrained uplink
- WireGuard and Synergy for administration and input sharing
- Tested VM backup and restoration workflows
- A planned bare-metal Windows option for games that are unsuitable for virtualization

The project is inspired by workload separation, virtualization, and infrastructure-as-code principles while remaining practical for everyday use.

## Current status

| Area | Status |
|---|---|
| Proxmox VE host | Operational |
| 4 TB NVMe VM storage | Operational |
| Ubuntu workstation VM | Operational |
| Windows 11 gaming VM | Operational |
| Parrot OS security environment | Operational |
| AMD Radeon Pro WX 3100 passthrough | Operational |
| NVIDIA GeForce RTX 4070 passthrough | Operational |
| Looking Glass B7 | Operational |
| Private routed VM network | Operational |
| WireGuard host management | Operational |
| Synergy keyboard and mouse sharing | Operational |
| VM backup and restore validation | Complete |
| Bare-metal Windows integration | Planned / in progress |
| Public documentation cleanup | In progress |

For the detailed dated build record, see:

- [Progress update — 3 August 2026](docs/progress/2026-08-03/README.md)
- [Progress update index](docs/progress/README.md)
- [Project roadmap](ROADMAP.md)

## Architecture

The platform uses Proxmox VE as a minimal bare-metal hypervisor.

```text
                         Household Wi-Fi
                                |
                         Proxmox VE host
                                |
             +------------------+------------------+
             |                  |                  |
      Ubuntu Workstation     Windows 11          Parrot OS
      AMD WX 3100            RTX 4070            Security VMs
      Primary desktop        On-demand gaming    Base + disposable
             |                  |
             +--- Looking Glass-+
             |
       WireGuard / Synergy
             |
          ThinkPad
```

The VM network is routed and NATed through the host because a normal Wi-Fi client connection cannot be used as a conventional Layer 2 bridge in the same way as Ethernet.

## Objectives

### Primary objectives

- Deploy Proxmox VE as the host operating system
- Create a dedicated Ubuntu workstation VM
- Create a dedicated Windows gaming VM
- Create a reusable Parrot OS security VM and disposable testing environment
- Implement dedicated GPU passthrough for both desktop VMs
- Provide a practical workflow for moving between Linux and Windows
- Document the architecture, implementation, failures, and lessons learned

### Secondary objectives

- Improve Linux and Proxmox administration skills
- Gain practical experience with virtualization and PCIe passthrough
- Develop routed virtual networking suitable for a Wi-Fi-only location
- Validate backups through real restoration
- Build a public project demonstrating infrastructure and systems engineering

## Hardware

| Component | Specification |
|---|---|
| CPU | AMD Ryzen 9 3900X |
| Motherboard | MSI MPG X570 Gaming Pro Carbon WiFi |
| RAM | 32 GB DDR4 |
| Windows GPU | NVIDIA GeForce RTX 4070 |
| Ubuntu GPU | AMD Radeon Pro WX 3100, 4 GB |
| Wireless adapter | Intel Wi-Fi 6 AX200 |
| Hypervisor storage | 500 GB NVMe SSD |
| Primary VM storage | 4 TB NVMe SSD |
| Bulk storage | 2 × 2 TB HDD |
| PSU | Approximately 800 W, 80 Plus Gold |
| Case | NZXT H510 Elite |
| Display | Huawei MateView GT 34-inch ultrawide |

The physical card layout differs from the original plan because both GPUs would not fit in the preferred arrangement:

```text
Top PCIe slot:     AMD Radeon Pro WX 3100
Lower PCIe slot:   NVIDIA GeForce RTX 4070
```

## Main virtual machines

### Ubuntu Workstation

The Ubuntu VM is the primary everyday desktop for development, university work, browsing, administration, and Linux applications.

Current headline configuration:

- VM ID 101
- OVMF / UEFI
- 4 virtual CPU cores
- 12 GB RAM
- 200 GB system disk
- AMD Radeon Pro WX 3100 passthrough
- QEMU Guest Agent
- SPICE audio
- WireGuard management connection
- Looking Glass client
- Synergy integration

### Windows 11 Gaming

The Windows VM is an on-demand environment for Windows applications and games that work correctly under virtualization.

Current headline configuration:

- VM ID 100
- OVMF / UEFI
- TPM 2.0
- 8 virtual CPU cores
- 8 GB RAM
- 300 GB system disk
- NVIDIA GeForce RTX 4070 passthrough
- Looking Glass B7 host
- IVSHMEM / KVMFR shared memory

A separate bare-metal Windows installation is being planned for anti-cheat-sensitive or virtualization-incompatible games. The VM remains useful for normal Windows workloads and maintenance tasks.


### Parrot OS Security Environment

Parrot OS provides a separated environment for cybersecurity tooling, practical labs, controlled testing, and disposable exercises without placing those tools in the primary Ubuntu workstation.

Current headline configuration:

- VM ID 102 — `parrot-security-base`
- 4 virtual CPU cores
- 8 GB RAM
- 80 GB system disk
- Reusable base image for security tooling and configuration
- VM ID 103 — `parrot-disposable-01`
- Disposable working clone for temporary exercises and testing
- No dedicated GPU passthrough required

The base VM is kept in a clean, reusable state. Disposable clones can be created for individual exercises and removed or reverted afterwards, reducing configuration drift and keeping the main workstation environment separate.

## Repository structure

```text
proxmox-workstation-platform/
├── README.md
├── ROADMAP.md
├── architecture/
│   └── proxmox_workstation_v1_architecture.svg
├── docs/
│   ├── gpu-passthrough-notes.md
│   ├── lessons-learned.md
│   ├── proxmox-setup.md
│   └── progress/
│       ├── README.md
│       └── 2026-08-03/
│           └── README.md
├── scripts/
└── screenshots/
```

## Learning outcomes

This project demonstrates practical experience with:

- Bare-metal virtualization
- Linux administration
- PCIe and GPU passthrough
- IOMMU and UEFI configuration
- Virtual storage management
- VM templates, cloning, and disposable environments
- Routed and NAT-based VM networking
- Wi-Fi-constrained infrastructure design
- WireGuard
- Backup and restoration
- Looking Glass shared-memory display
- Technical troubleshooting and documentation

## Security and publication

This is a public repository. Configuration examples must be sanitized before publication.

The repository must not contain passwords, private keys, WireGuard private keys, VPN credentials, API tokens, cookies, or personal data.

## Disclaimer

This is a personal learning and workstation project built on consumer hardware. It is not intended to provide enterprise high availability or production service guarantees.

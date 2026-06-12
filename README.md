# Proxmox Workstation Platform

## Overview

This project documents the design and implementation of a virtualized workstation platform built using Proxmox.

The primary goal is to separate day-to-day productivity workloads from gaming workloads while maintaining a single physical desktop. Rather than dual-booting between operating systems, the platform uses virtual machines with dedicated hardware resources assigned to each environment.

The project is inspired by principles of workload separation, virtualization, and infrastructure-as-code, while remaining practical for everyday use.

---

## Objectives

### Primary Objectives

* Deploy Proxmox as the host operating system
* Create a dedicated Ubuntu workstation VM for daily use
* Create a dedicated Windows gaming VM
* Implement GPU passthrough for near-native gaming performance
* Document the architecture and implementation process

### Secondary Objectives

* Improve Linux administration skills
* Gain practical experience with virtualization technologies
* Learn hardware passthrough and resource allocation
* Build a project suitable for demonstrating infrastructure and systems engineering concepts

---

## Architecture

```text
Physical Hardware
├── AMD Radeon Pro WX 3100
├── NVIDIA RTX 4070
├── Ryzen 9 3900X
├── 32GB DDR4 RAM
├── 500GB NVMe SSD
├── 4TB NVMe SSD
└── 2x 2TB HDD

        │

        ▼

Proxmox Hypervisor

        │

        ├── Ubuntu Workstation VM
        │   ├── Development
        │   ├── University Work
        │   ├── Browsing
        │   └── System Administration
        │
        └── Windows Gaming VM
            ├── GPU Passthrough
            ├── Steam
            └── Gaming Workloads
```

---

## Hardware

| Component          | Specification          |
| ------------------ | ---------------------- |
| CPU                | AMD Ryzen 9 3900X      |
| RAM                | 32GB DDR4              |
| Primary GPU        | NVIDIA RTX 4070        |
| Secondary GPU      | AMD Radeon Pro WX 3100 |
| Hypervisor Storage | 500GB NVMe SSD         |
| VM Storage         | 4TB NVMe SSD           |
| Bulk Storage       | 2x 2TB HDD             |

---

## Why Virtualization?

The traditional approach would be to dual-boot Linux and Windows. While simple, dual-booting requires a full system reboot whenever switching between operating systems.

This project explores an alternative architecture where:

* Ubuntu serves as the primary operating environment
* Windows exists as an on-demand gaming environment
* Resources can be allocated dynamically
* Workloads remain separated
* Additional environments can be introduced in the future

---

## Current Project Status

### Version 1

* [ ] Install Proxmox
* [ ] Configure storage
* [ ] Create Ubuntu workstation VM
* [ ] Create Windows gaming VM
* [ ] Configure GPU passthrough
* [ ] Test gaming performance
* [ ] Complete project documentation

### Future Enhancements

Potential future enhancements may include:

* Remote management capabilities
* Infrastructure monitoring
* Dashboard and automation tooling
* Media hosting services
* Additional virtualized workloads

These items are intentionally excluded from Version 1 to keep the project focused and achievable.

---

## Learning Outcomes

This project aims to provide hands-on experience with:

* Linux Administration
* Virtualization
* GPU Passthrough
* Storage Management
* Infrastructure Design
* Systems Engineering
* Documentation and Project Management

---

## Disclaimer

This project is intended as a personal learning platform and workstation environment. Design decisions prioritise practical usability, maintainability, and learning outcomes over enterprise-scale complexity.

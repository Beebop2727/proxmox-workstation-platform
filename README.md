# Proxmox Workstation Platform

> [!IMPORTANT]
> **Project status: Shelved — 4 September 2026**
>
> Active development of this project has ended. The workstation is no longer using **Proxmox VE** as its bare-metal host.
>
> The project is being superseded by an upcoming repository based around a **bare-metal Ryoku Linux workstation**, retaining virtual machines, KVM/QEMU, VFIO and GPU passthrough where they remain useful.
>
> Virtualisation itself is not being abandoned — the architectural change is to make Linux the primary host operating system rather than running the primary desktop inside a Proxmox virtual machine.

**Final phase:** V2 experimentation
**Project status:** Shelved / historical
**Active development ended:** 4 September 2026
**Last updated:** 4 September 2026

## Overview

This project documents the design, implementation and testing of a virtualised workstation and homelab platform built around **Proxmox VE**.

The original goal was to separate day-to-day Linux productivity from Windows gaming while retaining a single physical workstation.

Over time, the project expanded considerably and became a broader infrastructure experiment involving:

* Linux and Windows desktop virtual machines
* Dual-GPU PCIe passthrough
* Looking Glass
* Disposable cybersecurity environments
* OPNsense firewalling
* WireGuard management
* Routed and NAT-based virtual networking
* VM backup and recovery
* Monitoring infrastructure
* A separate Ubuntu Server support node
* Experimental Windows application integration

The platform successfully demonstrated that this architecture was technically viable.

However, the project also demonstrated that making the primary physical workstation a dedicated hypervisor introduced compromises for normal Linux desktop use, gaming and hardware access.

The next iteration therefore reverses the architecture:

```text
Proxmox-first model
-------------------

Hardware
   |
Proxmox VE
   |
   +-- Linux workstation VM
   +-- Windows VM
   +-- Security VMs
   +-- Infrastructure VMs


Successor model
---------------

Hardware
   |
Ryoku Linux
   |
   +-- Native Linux desktop
   +-- Native Linux applications / gaming
   |
   +-- KVM / QEMU / libvirt
          |
          +-- Windows VM
          +-- Security VMs
          +-- GPU-passthrough workloads
```

The useful virtualisation work developed here will therefore continue, but without requiring the workstation itself to operate primarily as a hypervisor.

---

## Successor project

A new repository will document the replacement workstation architecture.

The planned direction includes:

* **Ryoku Linux** as the bare-metal workstation operating system
* Hyprland-based desktop environment
* KVM/QEMU/libvirt virtualisation
* VFIO and PCIe passthrough
* GPU passthrough where useful
* Windows virtual machines
* Cybersecurity and disposable lab VMs
* Native Linux gaming where supported
* Direct access to the RTX 4070 from the Linux host
* Continued experimentation with isolated workloads and lab infrastructure

Several concepts from this project will carry directly into the successor:

* IOMMU configuration
* VFIO
* PCIe device isolation
* OVMF / UEFI guests
* GPU passthrough
* Virtual networking
* Disposable VMs
* Lab isolation
* WireGuard
* Documentation and decision records

**Successor repository:** Forthcoming.

Once the new repository is created, a direct link will be added here.

---

## Final documented state

The following table represents the final documented state of the Proxmox platform before retirement.

It is retained as a historical snapshot and does **not** indicate that these services remain operational.

| Area                                | Final documented state        |
| ----------------------------------- | ----------------------------- |
| Proxmox VE host                     | Operational before retirement |
| 4 TB NVMe VM storage                | Operational                   |
| Ubuntu workstation VM               | Operational                   |
| Windows 11 gaming VM                | Operational and play-tested   |
| Bare-metal Windows 11               | Operational                   |
| Parrot OS security environment      | Operational                   |
| AMD Radeon Pro WX 3100 passthrough  | Operational                   |
| NVIDIA GeForce RTX 4070 passthrough | Operational                   |
| Looking Glass B7                    | Operational                   |
| Private routed VM network           | Operational                   |
| Proxmox WireGuard management        | Operational                   |
| Ubuntu Server infrastructure host   | Operational                   |
| OPNsense firewall VM                | Operational                   |
| OPNsense WireGuard management       | Operational                   |
| VM backup / restore validation      | Complete                      |
| Grafana / Prometheus monitoring     | Operational                   |
| Isolated OPNsense lab network       | Operational                   |
| Dockur Windows environment          | Proof of concept operational  |
| WinApps / RemoteApp integration     | Unfinished at closure         |
| Uptime Kuma monitoring              | Unfinished at closure         |
| VLAN segmentation                   | Not implemented               |
| Wired Proxmox uplink                | Not implemented               |
| Automation tooling                  | Planned but not implemented   |

---

## Historical architecture

The final Proxmox-era platform evolved beyond the original single-host workstation into a small distributed homelab.

```text
                         Upstream LAN / Wi-Fi
                                  |
              +-------------------+-------------------+
              |                                       |
         Proxmox VE                           Ubuntu Server
       virtualization                      infrastructure host
              |                                       |
       +------+------+------+                   DNS / monitoring
       |      |      |      |                  WireGuard peer
    Ubuntu  Windows Parrot OPNsense
      VM      VM     VMs   firewall
       |       |
       +-- Looking Glass
              |
          Management
              |
           ThinkPad
```

Two separate WireGuard management paths were eventually used:

```text
Trusted client
      |
      +------ WireGuard ------ Proxmox
      |
      +------ WireGuard ------ OPNsense
```

The networking design relied heavily on routed and NAT-based virtual networking because the workstation was operating with a Wi-Fi uplink.

This worked, but it also became one of the architectural compromises that increased the complexity of using Proxmox as a normal workstation platform.

---

## Hardware

| Component          | Specification                        |
| ------------------ | ------------------------------------ |
| CPU                | AMD Ryzen 9 3900X                    |
| Motherboard        | MSI MPG X570 Gaming Pro Carbon WiFi  |
| RAM                | 64 GB DDR4                           |
| NVIDIA GPU         | GeForce RTX 4070                     |
| AMD GPU            | Radeon Pro WX 3100, 4 GB             |
| Wireless adapter   | Intel Wi-Fi 6 AX200                  |
| Hypervisor storage | 500 GB NVMe SSD                      |
| Primary VM storage | 4 TB NVMe SSD                        |
| Bulk storage       | 2 × 2 TB HDD                         |
| PSU                | EVGA 750 GQ — 750 W, 80 Plus Gold    |
| Case               | NZXT H510 Elite                      |
| Display            | Huawei MateView GT 34-inch ultrawide |

The physical GPU layout used during the project was:

```text
Top PCIe slot:     AMD Radeon Pro WX 3100
Lower PCIe slot:   NVIDIA GeForce RTX 4070
```

---

## Main virtual machines

### VM 100 — Windows 11

The Windows 11 VM provided the GPU-accelerated Windows environment.

Configuration included:

* OVMF / UEFI
* TPM 2.0
* RTX 4070 passthrough
* Looking Glass B7
* IVSHMEM / KVMFR integration
* Gaming and application testing

The VM was successfully play-tested.

A separate bare-metal Windows installation was retained for games or applications where native execution or anti-cheat compatibility was preferable.

---

### VM 101 — Ubuntu Workstation

The Ubuntu VM became the primary Linux desktop during the Proxmox phase.

Configuration included:

* OVMF / UEFI
* AMD Radeon Pro WX 3100 passthrough
* QEMU Guest Agent
* Development and administration tooling
* Looking Glass client
* Docker
* Nested KVM experimentation
* Dockur Windows testing
* WinApps / RemoteApp experiments

Plans later existed to migrate this desktop from GNOME to Hyprland.

That plan was superseded when the decision was made to move the Linux desktop back to bare metal entirely.

---

### VM 102 / 103 — Parrot OS

The security environment consisted of:

* A reusable Parrot OS base VM
* A disposable clone
* Isolated security tooling
* Controlled cybersecurity lab workloads

This concept remains useful and is expected to continue in some form within the successor architecture.

---

### VM 104 — OPNsense

OPNsense provided:

* Firewalling
* Lab DHCP
* Isolated VM networking
* WireGuard management
* The foundation for later segmentation work

Example historical addressing:

```text
WAN:        10.10.10.144/24
LAN:        10.10.99.1/24
WireGuard:  10.255.99.1/24
```

The OPNsense management tunnel was deliberately kept separate from the Proxmox management WireGuard network.

---

## Infrastructure host

An older Intel MacBook was repurposed as an Ubuntu Server infrastructure node.

Its roles included:

* SSH administration
* WireGuard
* Unbound DNS
* Secure upstream DNS
* Grafana
* Prometheus
* Node Exporter
* Supporting lab services

Separating supporting services from the main workstation proved to be one of the more successful architectural decisions in the project.

---

## Documentation

The repository is retained as a historical technical record.

* [Archived project roadmap](ROADMAP.md)
* [Progress log](docs/progress/README.md)
* [Decision records](docs/decisions/README.md)
* [Proxmox setup notes](docs/proxmox-setup.md)
* [GPU passthrough notes](docs/gpu-passthrough-notes.md)
* [Lessons learned](docs/lessons-learned.md)
* [Original V1 architecture diagram](architecture/proxmox_workstation_v1_architecture.svg)

Dated documents should be interpreted as snapshots of the platform at that point in its development.

Statements such as **planned**, **in progress**, or **next action** inside historical progress entries have intentionally not been rewritten.

Later records supersede those earlier plans.

---

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
│   │   ├── 2026-08-25-hyprland-decision.md
│   │   ├── 2026-08-27-obsidian-brain.md
│   │   └── 2026-09-04-shelve-proxmox-platform.md
│   ├── gpu-passthrough-notes.md
│   ├── lessons-learned.md
│   ├── proxmox-setup.md
│   └── progress/
│       ├── README.md
│       └── YYYY-MM-DD/
│           └── YYYY-MM-DD-description.md
└── scripts/
    └── scripts-readme.md
```

---

## Progress history

Major documented milestones include:

* **3 August** — Core Proxmox workstation rebuild and VM restoration
* **10 August** — Continued workstation integration and distributed infrastructure direction
* **11–12 August** — MacBook infrastructure experimentation and Ubuntu Server migration
* **13–14 August** — OPNsense deployment, WireGuard and firewall integration
* **15 August** — Grafana and Prometheus monitoring foundation
* **18 August** — Workstation upgraded to 64 GB RAM
* **19 August** — OPNsense-backed isolated cybersecurity lab
* **21 August** — Dockur Windows and WinApps proof of concept
* **24 August** — Further VM and workstation testing
* **25 August** — Planned Hyprland transition
* **27 August** — Documentation / research knowledge-base direction
* **4 September** — Proxmox workstation architecture retired in favour of a bare-metal Ryoku design

See [docs/progress/README.md](docs/progress/README.md) for the full indexed history.

---

## Unfinished work at closure

Several planned items were deliberately left unfinished when the architecture was retired.

These are retained here for historical context and are **not active tasks for this repository**.

### Networking

* Managed-switch deployment
* VLAN segmentation
* Trusted / server / management / security network separation
* Permanent wired Proxmox uplink

### Automation

* VM start / stop helpers
* Automated backup workflows
* Infrastructure health checks
* Dashboard integration
* Automated WireGuard and firewall verification

### Workstation

* Full WinApps / RemoteApp integration
* Final Hyprland deployment inside the Ubuntu VM
* Expanded gaming compatibility benchmarking
* Additional Looking Glass workflow refinement

### Infrastructure

* Complete Uptime Kuma deployment
* Expanded monitoring
* Automated power-management workflows
* Final distributed architecture diagram

Some of these ideas may reappear in the successor project where they remain useful.

---

## Learning outcomes

This project provided practical experience with:

* Bare-metal virtualisation
* Proxmox VE
* Linux administration
* PCIe passthrough
* VFIO
* IOMMU
* OVMF / UEFI
* Dual-GPU workstation design
* QEMU and KVM concepts
* Virtual storage management
* VM backup and restoration
* Disposable virtual machines
* Routed VM networking
* NAT
* Wi-Fi-constrained infrastructure
* WireGuard
* OPNsense
* Firewall troubleshooting
* Packet capture
* Connection tracking
* Looking Glass
* IVSHMEM / KVMFR
* Prometheus
* Grafana
* Distributed infrastructure design
* Technical documentation
* Architectural decision making

Perhaps the most important outcome was learning that a system can be **technically successful without being the best architecture for the intended workflow**.

Proxmox proved capable of running the workstation.

The eventual decision to remove it was therefore not caused by the experiment failing.

Instead, the experiment clarified the requirements of the next design.

---

## Project conclusion

The Proxmox Workstation Platform achieved its original purpose: to explore whether a single physical system could provide isolated Linux, Windows, cybersecurity and infrastructure environments while using dedicated GPU passthrough.

It could.

The project then grew far enough to reveal the trade-offs of making that virtualisation layer the foundation of the everyday desktop.

The successor architecture keeps the lessons that worked:

* virtual machines,
* isolation,
* passthrough,
* VFIO,
* lab environments,
* and infrastructure experimentation,

while returning the primary Linux desktop to bare metal.

For that reason, development of this repository ended on **4 September 2026**.

The repository remains public as a record of the experiment, its successes, its mistakes and the architectural decisions that followed.

---

## Security and publication

This is a public repository.

Configuration examples should remain sanitised and must not contain:

* Passwords
* Private keys
* WireGuard private keys
* VPN credentials
* API tokens
* Cookies
* Recovery codes
* Personally identifying data
* Unnecessary public IP addresses

Private RFC1918 addresses may be documented where useful for explaining the historical network design.

---

## Disclaimer

This was a personal learning, workstation and homelab project built using consumer hardware.

It was not intended to provide enterprise high availability or production service guarantees.

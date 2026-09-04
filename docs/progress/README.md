# Project Progress Log

> [!NOTE]
> **Historical project record**
>
> Active development of the Proxmox Workstation Platform ended on **4 September 2026**.
>
> The entries in this directory are preserved as dated snapshots of the project and have intentionally not been rewritten to reflect later decisions.

This directory records the development of the Proxmox Workstation Platform from its initial workstation build through networking, GPU passthrough, firewalling, monitoring and desktop-integration experiments.

Earlier entries may contain incomplete tasks, temporary architecture decisions or statements describing work as **planned** or **in progress**.

Those statements describe the state of the project at that date.

The final project decision supersedes any remaining open work.

## Entries

| Date                                                                         | Phase               | Summary                                                                                         |
| ---------------------------------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------- |
| [2026-08-03](2026-08-03/2026-08-03-document-proxmox-progress-1.md)           | V1 / V1.5           | Host rebuild, VM restoration, dual-GPU workstation, Looking Glass and routed Wi-Fi networking   |
| [2026-08-10](2026-08-10/2026-08-10-proxmox-continuation.md)                  | V1.5 / V2           | Proxmox management, WireGuard and distributed infrastructure direction                          |
| [2026-08-11](2026-08-11/2026-08-11-macbook-infrastructure.md)                | Infrastructure      | MacBook infrastructure testing and macOS network-filtering investigation                        |
| [2026-08-12](2026-08-12/2026-08-12-macbook-server.md)                        | Infrastructure      | MacBook migration from macOS to Ubuntu Server                                                   |
| [2026-08-13 — MacBook](2026-08-13/2026-08-13-macbook-update.md)              | Infrastructure      | Ubuntu Server Wi-Fi, SSH, WireGuard, Unbound and secure DNS                                     |
| [2026-08-13 — OPNsense](2026-08-13/2026-08-13-Opnsense-firewall.md)          | Networking          | OPNsense VM deployment, WAN/LAN configuration, DHCP and WireGuard                               |
| [2026-08-14](2026-08-14/2026-08-14-OPNsense-firewall-cleanup.md)             | Networking          | OPNsense WireGuard management, persistent DNAT and firewall cleanup                             |
| [2026-08-15](2026-08-15/2026-08-15-monitoring-progress.md)                   | Monitoring          | Grafana, Prometheus and Node Exporter monitoring foundation                                     |
| [2026-08-18](2026-08-18/2026-08-18-ram-successfully-installed-and-tested.md) | Hardware            | Workstation upgraded from 32 GB to 64 GB RAM                                                    |
| [2026-08-19](2026-08-19/2026-08-19-OPNsense-and-lab-configuration.md)        | Networking / lab    | OPNsense lab isolation and Uptime Kuma experimentation                                          |
| [2026-08-21](2026-08-21/2026-08-21-dockur-winapps-progress.md)               | Integration         | Dockur Windows proof of concept and WinApps experimentation                                     |
| [2026-08-24](2026-08-24/2026-08-24-vm-additional-testing.md)                 | VM testing          | Additional virtual-machine and workstation testing                                              |
| [2026-08-25](2026-08-25/2026-08-25-hyprland-transition.md)                   | Desktop             | Planned migration of the Ubuntu workstation VM from GNOME to Hyprland                           |
| **2026-09-04**                                                               | **Project closure** | **Proxmox workstation architecture shelved in favour of a bare-metal Ryoku workstation design** |

## Final outcome

The project successfully demonstrated:

* GPU passthrough
* Windows and Linux workstation VMs
* Looking Glass
* OPNsense
* WireGuard
* Routed virtual networking
* Security VMs
* Backup and restoration
* Monitoring infrastructure
* Distributed supporting services

The architecture was eventually retired because running the primary Linux desktop as a guest introduced unnecessary compromises for the workstation's day-to-day role.

Future virtualisation work will continue from a bare-metal Linux host instead.

## Related documentation

* [Root project README](../../README.md)
* [Archived roadmap](../../ROADMAP.md)
* [Decision records](../decisions/README.md)
* [Proxmox setup notes](../proxmox-setup.md)
* [GPU passthrough notes](../gpu-passthrough-notes.md)
* [Lessons learned](../lessons-learned.md)

## Historical-document policy

Progress entries should remain historical unless:

* A factual error needs correcting
* Sensitive information needs removing
* A broken link needs repairing

Later decisions should supersede earlier plans rather than rewriting the original record.

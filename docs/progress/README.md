# Project progress log

This directory contains dated snapshots of the Proxmox Workstation Platform.

The root [`README.md`](../../README.md) describes the current project at a high level. These entries preserve build history, including completed work, design changes, faults, recoveries, and next actions.

## Entries

| Date | Phase | Summary |
|---|---|---|
| [2026-08-03](2026-08-03/2026-08-03-document-proxmox-progress-1.md) | V1 / V1.5 | Host rebuild, network bootstrap, restored VMs, dual-GPU workstation, Looking Glass, routed Wi-Fi networking, and initial bare-metal Windows planning |
| [2026-08-10](2026-08-10/2026-08-10-proxmox-continuation.md) | V1.5 / V2 direction | Wi-Fi-based Proxmox management, expanded WireGuard management, and separate infrastructure-host direction |
| [2026-08-11](2026-08-11/2026-08-11-macbook-infrastructure.md) | Infrastructure | macOS infrastructure-service testing and CFIL troubleshooting |
| [2026-08-12](2026-08-12/2026-08-12-macbook-server.md) | Infrastructure | MacBook migration from macOS to Ubuntu Server and offline Wi-Fi recovery work |
| [2026-08-13 — MacBook](2026-08-13/2026-08-13-macbook-update.md) | Infrastructure | Ubuntu Server Wi-Fi, SSH, WireGuard, Unbound, and secure DNS brought operational |
| [2026-08-13 — OPNsense](2026-08-13/2026-08-13-Opnsense-firewall.md) | Networking / firewall | OPNsense VM deployment, WAN/LAN configuration, DHCP, GUI access, and WireGuard foundation |
| [2026-08-14](2026-08-14/2026-08-14-OPNsense-firewall-cleanup.md) | Networking / firewall | Completed OPNsense WireGuard management, persistent DNAT, reboot conntrack recovery, and closure of earlier loose ends |
| [2026-08-15](2026-08-15/2026-08-15-monitoring-progress.md) | Monitoring | Grafana, Prometheus and Node Exporter monitoring foundation deployed on the Ubuntu Server infrastructure host |
| [2026-08-18](2026-08-18/2026-08-18-ram-successfully-installed-and-tested.md) | Hardware | Workstation memory upgraded from 32 GB to 64 GB and initial stability testing completed |
| [2026-08-19](2026-08-19/2026-08-19-OPNsense-and-lab-configuration.md) | Networking / monitoring | OPNsense lab isolation refined, memory increased, and Uptime Kuma deployment started |
| [2026-08-21](2026-08-21/2026-08-21-dockur-winapps-progress.md) | Workstation integration | Dockur Windows proof of concept deployed inside Ubuntu VM 101 in preparation for WinApps / RemoteApp integration |

## Related documentation

- [Root project README](../../README.md)
- [Roadmap](../../ROADMAP.md)
- [Proxmox setup notes](../proxmox-setup.md)
- [GPU passthrough notes](../gpu-passthrough-notes.md)
- [Lessons learned](../lessons-learned.md)
- [Windows environment decision](../decisions/2026-08-05-windows-environments.md)
- [Distributed infrastructure decision](../decisions/2026-08-10-distributed-infrastructure.md)

## Naming convention

Progress entries now use descriptive filenames inside a dated directory:

```text
docs/progress/YYYY-MM-DD/YYYY-MM-DD-short-description.md
```

This keeps the date visible in both the directory and filename while allowing multiple progress entries on the same day.

Each entry should record:

- What was operational at the start
- Work completed on that date
- Important commands or configuration changes
- Problems encountered
- Decisions and trade-offs
- Current state
- Next actions
- Explicit closure or supersession of important earlier open items

Older entries should remain historical unless a factual correction or repair is required.

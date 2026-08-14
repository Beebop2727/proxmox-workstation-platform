# Project progress log

This directory contains dated snapshots of the Proxmox Workstation Platform.

The root `README.md` describes the current project at a high level. These entries preserve build history, including completed work, design changes, faults, recoveries, and next actions.

## Entries

| Date | Phase | Summary |
|---|---|---|
| [2026-08-03](2026-08-03/README.md) | V1 / V1.5 | Host rebuild, network bootstrap, restored VMs, dual-GPU workstation, Looking Glass, routed Wi-Fi networking, and initial bare-metal Windows planning |
| [2026-08-10](2026-08-10/README.md) | V1.5 / V2 direction | Wi-Fi-based Proxmox management, expanded WireGuard management, and separate infrastructure-host direction |
| [2026-08-11](2026-08-11/2026-08-11.md) | Infrastructure | macOS infrastructure-service testing and CFIL troubleshooting |
| [2026-08-12](2026-08-12/2026-08-12-macbook-server.md) | Infrastructure | MacBook migration from macOS to Ubuntu Server and offline Wi-Fi recovery work |
| [2026-08-13 — MacBook](2026-08-13/2026-08-13-macbook-update.md) | Infrastructure | Ubuntu Server Wi-Fi, SSH, WireGuard, Unbound, and secure DNS brought operational |
| [2026-08-13 — OPNsense](2026-08-13/2026-08-13-Opnsense-firewall.md) | Networking / firewall | OPNsense VM deployment, WAN/LAN configuration, DHCP, GUI access, and WireGuard foundation |
| [2026-08-14](2026-08-14/README.md) | Networking / firewall | Completed OPNsense WireGuard management, persistent DNAT, reboot conntrack recovery, and closure of earlier loose ends |

## Naming convention

New entries should normally use:

```text
docs/progress/YYYY-MM-DD/README.md
```

Some earlier entries use descriptive filenames inside the dated directory. They are retained to avoid rewriting historical paths.

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

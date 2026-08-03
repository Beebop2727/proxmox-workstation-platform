# Project progress log

This directory contains dated snapshots of the Proxmox Workstation Platform.

The root `README.md` describes the current project at a high level. These entries preserve the build history, including completed work, design changes, faults, recoveries, and next actions.

## Entries

| Date | Phase | Summary |
|---|---|---|
| [2026-08-03](2026-08-03/README.md) | V1 / V1.5 | Host rebuild, network bootstrap, restored VMs, dual-GPU workstation, Looking Glass, routed Wi-Fi networking, and the current bare-metal Windows extension |

## Naming convention

New entries should use the following structure:

```text
docs/progress/YYYY-MM-DD/README.md
```

Each entry should record:

- What was operational at the start
- Work completed on that date
- Important commands or configuration changes
- Problems encountered
- Decisions and trade-offs
- Current state
- Next actions

Older entries should remain unchanged unless a factual correction is required. New progress should normally be recorded in a new dated directory.

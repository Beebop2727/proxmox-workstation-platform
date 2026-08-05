# Windows Environment Architecture Decision

> **Date:** 5 August 2026
> **Status:** Under evaluation

## Context

The platform originally uses a **Windows 11 gaming VM** with:

* NVIDIA RTX 4070 passthrough
* Looking Glass
* OVMF / UEFI
* TPM 2.0

Minecraft has been successfully play-tested, confirming functional GPU acceleration and stable gameplay.

A separate **bare-metal Windows 11 installation** has been added for native performance and improved compatibility with anti-cheat-protected or virtualization-sensitive games.

## Present Configuration

Both Windows environments will be retained for now.

### Bare-metal Windows

* Native gaming performance
* Better anti-cheat compatibility
* Direct hardware access

### Windows Gaming VM

* Compatible games and Windows applications
* GPU passthrough and Looking Glass testing
* Snapshot-based software and driver testing
* Windows tasks without rebooting the workstation

## Future Plans

The long-term role of each environment may change as the project develops.

The gaming VM could eventually become a dedicated **Windows testing and compatibility environment**. Bare-metal Windows could also be replaced by a Linux gaming distribution such as **Bazzite** or **CachyOS**.

> No permanent decision has been made yet for the viability of the gaming VM or bare-metal solution for gaming.

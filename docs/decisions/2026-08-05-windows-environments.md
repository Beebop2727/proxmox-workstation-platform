# Windows Environment Architecture Decision

> **Date:** 5 August 2026  
> **Status:** Implemented; long-term roles still under evaluation

## Context

The platform uses a Windows 11 VM with RTX 4070 passthrough and Looking Glass.

A separate bare-metal Windows 11 installation was added for native performance and improved compatibility with anti-cheat-protected or virtualization-sensitive games.

## Present Configuration

Both environments are retained.

### Bare-metal Windows

- Native gaming performance
- Better anti-cheat compatibility
- Direct hardware access
- Operational alongside Proxmox through the existing boot arrangement

### Windows VM

- Compatible games and Windows applications
- GPU passthrough / Looking Glass testing
- Snapshot-based driver and software testing
- Windows tasks without rebooting the host into bare-metal Windows

## Implementation update — 14 August 2026

The earlier open loop around whether bare-metal Windows would actually be installed has been closed: the native Windows environment is operational.

The remaining decision is narrower: determine the long-term role of VM 100 as Linux gaming support improves and workload requirements change.

No requirement currently exists to remove either environment.

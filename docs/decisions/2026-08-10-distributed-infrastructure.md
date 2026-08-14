# Distributed Infrastructure Decision

> **Date:** 10 August 2026  
> **Status:** Accepted and in implementation

## Introduce a Separate Infrastructure Host

The platform will evolve toward a distributed infrastructure model rather than placing all supporting services directly on the Proxmox host.

An older MacBook is being repurposed as a dedicated infrastructure/support host.

### Rationale

Moving supporting services away from Proxmox provides:

- Reduced resource contention
- Better separation between infrastructure and workloads
- Management services that can remain independent of workstation VMs
- A useful second Linux system for networking and security experiments

## Retain Proxmox as the Primary Hypervisor

The Proxmox host remains responsible for virtualization and high-intensity workloads.

The MacBook and ThinkPad will not be added as Proxmox cluster nodes.

```text
Proxmox  -> Virtualization / high-intensity workloads
MacBook  -> Infrastructure / supporting services
ThinkPad -> Administration / management workstation
```

## Use WireGuard for Management

WireGuard remains the preferred logical management network where useful.

The existing Proxmox management WireGuard network is kept separate from the later OPNsense management tunnel.

This avoids coupling firewall management to the hypervisor's existing tunnel.

## Implementation update — 14 August 2026

Several assumptions from 10 August changed during implementation:

- macOS was abandoned after repeated Content Filtering / firewall problems.
- The MacBook now runs Ubuntu Server.
- Wi-Fi, SSH, WireGuard, and lab DNS are working on the server.
- Unbound replaced the earlier AdGuard Home direction for lab DNS.
- The earlier Raspberry Pi control-plane idea is no longer required for the current architecture.
- OPNsense has been added as a dedicated firewall VM with its own WireGuard management tunnel.

The distributed-infrastructure decision itself remains valid; the implementation is now Linux-based and includes a dedicated firewall layer.

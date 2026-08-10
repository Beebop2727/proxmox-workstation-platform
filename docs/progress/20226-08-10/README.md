# Progress Update — 10 August 2026

## Overview

This session extended the Proxmox workstation platform beyond the original single-host workstation design, establishing a more practical management and infrastructure architecture around the Proxmox host.

The main focus was improving remote administration over Wi-Fi and integrating a separate MacBook as a potential infrastructure and management host.

## Completed

### Wi-Fi-based Proxmox Management

The Proxmox host was successfully operated without its physical Ethernet connection.

Current Proxmox networking:

* Wi-Fi uplink: `wlo1`
* Proxmox Wi-Fi address: `192.168.1.94/24`
* VM bridge: `vmbr0`
* Private VM network: `10.10.10.0/24`
* Proxmox bridge address: `10.10.10.1/24`

The existing NAT/routed VM networking continues to operate over the Wi-Fi uplink rather than requiring a conventional Ethernet bridge.

### WireGuard Management Network

WireGuard was extended to support multiple management peers.

Current management network:

* Proxmox WireGuard: `172.31.255.1/24`
* ThinkPad: `172.31.255.2/24`
* MacBook: `172.31.255.3/24`

Both the ThinkPad and MacBook successfully established WireGuard handshakes with the Proxmox host.

This provides a dedicated management path independent of the VM network.

### MacBook Management Host

An Intel MacBook running macOS 15.7.7 was integrated into the infrastructure.

The MacBook is intended to become a separate infrastructure/management host rather than another Proxmox node.

Potential future services include:

* Infrastructure dashboard
* DNS services
* VPN/remote-access services
* Wake-on-LAN
* Monitoring
* Honeypots
* Security tooling and supporting services

The intention is to move supporting infrastructure workloads away from the Proxmox host so that Proxmox resources can remain focused on high-intensity virtual machines.

### Remote SSH Administration

SSH access from the ThinkPad to the MacBook was successfully established.

macOS initially accepted the TCP connection but reset SSH sessions because the Application Firewall was blocking:

`/usr/libexec/sshd-session`

The firewall configuration was investigated using `socketfilterfw` and macOS unified logging.

The specific application rule was corrected so that `sshd-session` permits incoming connections while the macOS Application Firewall remains enabled.

SSH access was subsequently confirmed working.

### Security / Firewall Investigation

The SSH issue provided useful practical experience with macOS networking and firewall behaviour.

The investigation identified the following:

* `sshd` was permitted.
* `sshd-keygen-wrapper` was permitted.
* `sshd-session` was blocked.
* macOS kernel logs showed `CFIL_OP_DROP` events against `sshd-session`.
* Unblocking `sshd-session` restored SSH connectivity.

An experimental `pf` configuration was briefly attempted but was removed after failing syntax validation. No invalid custom `pf` rule was left in the configuration.

## Current Architecture Direction

The platform is evolving from a single Proxmox workstation into a small distributed homelab/infrastructure environment:

```text
                         Household Wi-Fi
                              |
              +---------------+---------------+
              |               |               |
          Proxmox           MacBook         ThinkPad
        192.168.1.94      192.168.1.67     192.168.1.70
              |               |               |
              |          Infrastructure      Management
              |             Services
              |
          vmbr0
       10.10.10.0/24
              |
       +------+------+------+
       |      |      |      |
     Ubuntu Windows Parrot  ...
       VM      VM     VMs

                 WireGuard
              172.31.255.0/24
                 |
       +---------+---------+
       |                   |
   ThinkPad             MacBook
   .2 peer              .3 peer
```

## Design Decision

The MacBook and ThinkPad will **not** be added as Proxmox cluster nodes.

Proxmox remains dedicated to virtualization, while the MacBook becomes a potential supporting infrastructure host and the ThinkPad remains the primary administration workstation.

This keeps the Proxmox host focused on resource-intensive workloads while allowing supporting services to be distributed across separate hardware.

## Next Steps

* Build the MacBook infrastructure service stack.
* Investigate lightweight containerisation on macOS.
* Establish dedicated DNS for security/testing VMs.
* Implement Wake-on-LAN management.
* Build infrastructure monitoring.
* Investigate secure remote access from outside the local network.
* Evaluate VLAN segmentation for infrastructure and security services.
* Develop honeypot/security-testing services on the MacBook.
* Document the expanded architecture.
* Review whether additional services should be migrated away from Proxmox.

## Status

**V1:** Operational

**V1.5:** Infrastructure integration and refinement in progress

**New milestone:** Distributed management/infrastructure architecture established.

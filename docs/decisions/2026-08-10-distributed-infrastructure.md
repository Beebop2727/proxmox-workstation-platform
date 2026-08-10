# Distributed Intrastructure Decision

> **Date:** 10 August 2026
> **Status:** Accepted

## Introduce a Separate Infrastructure Host

**Status:** Accepted

The platform will evolve toward a distributed infrastructure model rather than placing all supporting services directly on the Proxmox host.

The MacBook will be repurposed as a dedicated infrastructure and management host where practical.

### Rationale

The Proxmox machine should remain primarily responsible for running high-intensity virtual machines and security workloads.

Moving supporting services to separate hardware provides:

* Reduced resource contention on Proxmox.
* Greater separation between infrastructure and workloads.
* Improved availability of management services when VMs are stopped.
* A realistic opportunity to experiment with distributed infrastructure.
* Additional hardware for security experimentation without affecting the primary hypervisor.

### Planned MacBook Services

The MacBook is intended to host selected infrastructure services including:

* DNS
* VPN / remote access
* Infrastructure dashboard
* Wake-on-LAN
* Monitoring
* Honeypots
* Supporting security services

The exact service allocation will be determined during implementation.

---

## Decision: Retain Proxmox as the Primary Hypervisor

**Status:** Accepted

The Proxmox host will remain dedicated primarily to virtualization and high-intensity workloads.

The existing VM architecture will therefore remain centred around Proxmox rather than converting the MacBook or ThinkPad into additional Proxmox cluster nodes.

### Rationale

Adding the MacBook or ThinkPad as Proxmox cluster nodes would introduce unnecessary clustering complexity without providing a meaningful benefit for the current platform.

The machines have distinct roles:

```text
Proxmox  → Virtualization / high-intensity workloads
MacBook  → Infrastructure / supporting services
ThinkPad  → Administration / management workstation
```

---

## Decision: Use WireGuard as the Management Network

**Status:** Accepted

WireGuard will provide the dedicated management network between the infrastructure devices.

Current addressing:

```text
172.31.255.1  Proxmox
172.31.255.2  ThinkPad
172.31.255.3  MacBook
```

The existing Wi-Fi network remains the physical transport, while WireGuard provides logical separation for management traffic.

### Rationale

This avoids requiring dedicated Ethernet connectivity for every management device while maintaining a predictable private management network.

It also provides a foundation for future remote-access capabilities without exposing management services directly to the local network or Internet.

---

## Decision: Use Wi-Fi Instead of Purchasing Additional Docking Hardware

**Status:** Accepted

The Proxmox host will continue using its Wi-Fi uplink for the current platform rather than requiring another Thunderbolt/Ethernet dock.

### Rationale

The existing Wi-Fi connection is sufficient for the current management and VM networking requirements.

Avoiding additional hardware also keeps the platform inexpensive while the architecture is still evolving.

## Architectural Direction

These decisions establish the next architectural phase of the project:

```text
                    Network
                       |
        +--------------+--------------+
        |              |              |
     Proxmox         MacBook       ThinkPad
   Hypervisor      Infrastructure   Management
        |              |              |
     VMs/CTs       Services       Administration
        |
   High-intensity
    workloads

              WireGuard Management
                 172.31.255.0/24
```

This represents a shift from a **single-host workstation platform** toward a **distributed homelab infrastructure platform**, while retaining Proxmox as the central virtualization layer.

# ThinkPad VM Testing and Virtual Display Evaluation

**Date:** 24 August 2026
**Status:** Testing complete / architecture under evaluation

## Overview

A local QEMU/KVM virtual machine was configured on the ThinkPad to evaluate lightweight virtualisation and remote-display options outside the main Proxmox workstation.

The test was primarily intended to create an isolated Parrot OS environment for opening untrusted content, but it also provided a useful comparison against the existing Proxmox desktop VM experience.

## ThinkPad Test Environment

The VM was configured using:

* QEMU/KVM
* libvirt
* `virt-install` / `virsh`
* Parrot OS with LXQt
* 6 vCPUs
* 8 GB RAM
* 80 GB QCOW2 virtual disk
* VirtIO devices
* libvirt NAT networking
* AppArmor confinement
* No host filesystem sharing
* No direct USB or GPU passthrough

Networking was initially disabled for isolation and later enabled through libvirt's NAT network to allow normal web access and software downloads.

## Virtual Display Testing

Several display methods were evaluated.

### VNC / noVNC

The VM was initially exposed through QEMU's local VNC server and accessed using both `remote-viewer` and noVNC.

Performance was unexpectedly responsive, with very little perceived input latency despite using a virtual display rather than GPU passthrough.

This raised an important question regarding the current Proxmox workstation architecture: some perceived sluggishness may originate from the existing display/input path rather than the VM workloads themselves.

### SPICE

The VM was subsequently migrated to SPICE for improved desktop integration and audio experimentation.

VirtIO GPU acceleration and VirGL were also investigated, allowing the guest to use the ThinkPad's Intel Arc render node while retaining a virtual GPU rather than passing physical graphics hardware directly into the guest.

LXQt fractional/global scaling was enabled to improve usability on the ThinkPad's high-resolution display.

## Audio Testing

Several approaches were investigated for routing guest audio through the Ubuntu host.

Testing included:

* SPICE audio
* virtual ICH9 audio
* PipeWire integration
* direct QEMU-to-host PipeWire routing
* AppArmor permissions required by the PipeWire backend

Further refinement is still required before this configuration is considered final.

## Potential Proxmox Architecture Change

The ThinkPad testing demonstrated that virtual displays may be sufficiently responsive for general workstation and lab VM usage.

A future revision of the Proxmox workstation may therefore move toward:

```text
Physical Proxmox Host
        │
        ├── Ubuntu Workstation VM
        │     └── Physical GPU passthrough
        │
        ├── Windows VM
        ├── Parrot Base VM
        ├── Parrot Disposable VM
        ├── OPNsense
        └── Additional Lab VMs
                 │
                 ▼
          Virtual Displays
                 │
          Browser / Viewer
                 │
                 ▼
        Ubuntu Workstation
```

Under this model, the Ubuntu workstation would remain the primary desktop while other VMs could be accessed through virtual screens using technologies such as:

* Proxmox noVNC
* SPICE
* RDP
* Apache Guacamole
* other browser-based remote desktop solutions

This could substantially simplify the current workstation experience by reducing reliance on physical GPU outputs, Looking Glass, display switching and input-sharing mechanisms for VMs that do not require native GPU performance.

## Current Decision

No changes will be made to the primary Proxmox display architecture yet.

The ThinkPad testing has instead established **virtual displays as a viable option for further experimentation**.

Future testing should compare:

1. Proxmox noVNC with VirtIO display
2. SPICE performance
3. RDP for Windows and Linux desktop VMs
4. Apache Guacamole as a unified browser-based VM portal
5. Existing GPU passthrough / Looking Glass performance

GPU passthrough will remain appropriate for workloads such as gaming and GPU-intensive applications, while virtual displays may become the preferred solution for general-purpose and cybersecurity lab VMs.

## Result

**✅ QEMU/KVM VM successfully deployed on ThinkPad**
**✅ Parrot OS tested with isolated and NAT networking**
**✅ VNC/noVNC demonstrated very low perceived latency**
**✅ SPICE and VirtIO/VirGL investigated**
**✅ Virtual display architecture identified as a potential future Proxmox direction**
**🔬 Further Proxmox-side testing planned before adoption**

# V1 / V1.5 progress update

**Date:** 3 August 2026  
**Project:** Proxmox Workstation Platform  
**State:** Core platform operational; integration, recovery, and storage work ongoing

## Summary

The project has progressed beyond the original Version 1 proof of concept.

The machine now operates as a Proxmox-based workstation with dedicated GPUs assigned to the Ubuntu and Windows desktop VMs. Looking Glass provides low-latency access to the Windows VM from Ubuntu, while WireGuard and Synergy support administration and input sharing.

On 3 August 2026, the Proxmox host was installed or rebuilt on the 500 GB NVMe, temporary internet access was established through a ThinkPad dock, the Intel AX200 Wi-Fi hardware was confirmed, and previously backed-up virtual machines were restored to the primary 4 TB NVMe storage.

The current V1.5 work is focused on making the system reliable as an everyday workstation and preparing an optional bare-metal Windows installation for games that are unsuitable for virtualization.

## Hardware platform

| Component | Current hardware |
|---|---|
| CPU | AMD Ryzen 9 3900X — 12 cores / 24 threads |
| Motherboard | MSI MPG X570 Gaming Pro Carbon WiFi |
| Memory | 32 GB DDR4 |
| Ubuntu GPU | AMD Radeon Pro WX 3100, 4 GB |
| Windows GPU | NVIDIA GeForce RTX 4070 |
| Hypervisor drive | 500 GB NVMe SSD |
| Primary VM drive | 4 TB NVMe SSD |
| Bulk drives | 2 × 2 TB HDD |
| Wireless adapter | Intel Wi-Fi 6 AX200 |
| Case | NZXT H510 Elite |
| CPU cooling | Front-mounted AIO |
| Main display | Huawei MateView GT 34-inch ultrawide |

## Physical GPU layout

The preferred slot arrangement was not physically possible because of card size and cooler clearance.

The working installation is:

```text
Top PCIe slot:     AMD Radeon Pro WX 3100
Lower PCIe slot:   NVIDIA GeForce RTX 4070
```

The motherboard PCIe configuration was adjusted so that both cards could operate in the available slots.

## BIOS and firmware work

The following settings were enabled:

- SVM Mode
- IOMMU
- Above 4G Decoding
- UEFI boot
- Dual-slot PCIe lane configuration

The IOMMU setting was explicitly enabled rather than left on an automatic setting.

The local console keyboard uses a US layout. Debian's keyboard configuration was updated after the installer initially applied a UK layout.

## Proxmox host

Current recorded software state:

| Component | Version / state |
|---|---|
| Proxmox manager | 9.2.2 |
| Kernel | 7.0.2-6-pve |
| Host installation drive | 500 GB NVMe |
| Main VM storage | `nvme4tb-vg` |
| Wi-Fi driver | `iwlwifi` |
| Wireless device | Intel AX200 |

The host is intended to remain minimal. Normal browsing, development, and desktop applications belong inside guests rather than on the hypervisor.

## Network bootstrap on 3 August 2026

Direct router Ethernet access was unavailable. The initial Proxmox bridge still used an address from the normal household LAN, but its physical Ethernet link was connected to a ThinkPad rather than the router.

A temporary routed connection was created:

```text
Household Wi-Fi
      |
Ubuntu ThinkPad
      |
NetworkManager: Shared to other computers
      |
ThinkPad dock Ethernet — 10.42.0.1/24
      |
Proxmox Ethernet bridge — 10.42.0.2/24
```

After placing the Proxmox bridge in the `10.42.0.0/24` subnet and using the ThinkPad as the gateway, internet access was restored.

This temporary connection allowed package updates and installation of the tools required to continue configuring the AX200.

### Important networking lesson

A Wi-Fi client interface cannot normally be attached to a standard Linux bridge in the same manner as Ethernet because of the usual 802.11 three-address limitation.

The durable design is therefore:

```text
Intel AX200 Wi-Fi uplink
          |
     Proxmox routing/NAT
          |
   Private virtual bridge
          |
      Guest VMs
```

The VM network uses a private subnet and reaches the household network through host routing and NAT.

## Primary virtual machines

### VM 100 — Windows 11 Gaming

| Setting | Current state |
|---|---|
| VM ID | 100 |
| Name | Windows-11-Gaming |
| Firmware | OVMF / UEFI |
| TPM | TPM 2.0 |
| CPU allocation | 8 virtual CPU cores |
| Memory | 8 GB currently; 16 GB previously tested |
| System disk | 300 GB |
| GPU | NVIDIA GeForce RTX 4070 passthrough |
| Display integration | Looking Glass B7 |
| Shared memory | IVSHMEM, currently 64 MB |

The Windows VM is used for Windows-only applications and games that operate correctly under virtualization.

### VM 101 — Ubuntu Workstation

| Setting | Current state |
|---|---|
| VM ID | 101 |
| Name | Ubuntu-Workstation |
| Firmware | OVMF / UEFI |
| CPU allocation | 4 virtual CPU cores |
| Memory | 12 GB |
| System disk | 200 GB |
| GPU | AMD Radeon Pro WX 3100 passthrough |
| Guest integration | QEMU Guest Agent |
| Audio | SPICE audio |
| Primary role | Daily workstation |

The Ubuntu VM is the main environment for development, university work, browsing, system administration, and general use.

Its current desktop environment includes GNOME, Wayland, PipeWire, development tools, VPN software, custom desktop extensions, and the Looking Glass client.

## Looking Glass

Looking Glass B7 provides a low-latency display path from the RTX 4070-backed Windows VM into Ubuntu.

Current implementation details:

- Looking Glass B7 host inside Windows
- Looking Glass client inside Ubuntu
- KVMFR DKMS module version 0.0.12
- IVSHMEM tested at 64 MB and 128 MB
- Current shared-memory allocation: 64 MB
- Escape key: Right Ctrl

This avoids repeatedly changing the monitor input and makes Windows behave more like an on-demand application environment.

Keyboard and mouse capture behaviour is still being refined.

## Synergy and WireGuard

Synergy 3 is used to share keyboard and mouse input between the Ubuntu workstation and the ThinkPad.

Because the Ubuntu VM sits behind the routed virtual network, Synergy traffic is forwarded through the Proxmox host.

WireGuard provides a separate management route between the Proxmox host and Ubuntu VM. The implementation uses private addresses and no private key material is stored in this repository.

## Storage

### 500 GB NVMe

Used for Proxmox VE and core hypervisor storage.

### 4 TB NVMe

Used as the primary VM storage pool:

```text
nvme4tb-vg
```

Restored principal guest disks:

| VM | Disk size |
|---|---:|
| Windows 11 Gaming | 300 GB |
| Ubuntu Workstation | 200 GB |

### 2 × 2 TB HDD

Reserved for future bulk storage and backup use. They are not required for the core V1 workstation and can be integrated after the main platform is stable.

## Backup and restoration

Before modifying the 4 TB NVMe layout, the VMs were backed up.

The restoration process was then tested rather than treating the existence of backup files as proof that recovery worked.

Confirmed restored guests include:

```text
VM 100  Windows-11-Gaming
VM 101  Ubuntu-Workstation
```

Additional internal templates and test guests are outside the public project's main scope.

The project also uses guest snapshots and a Timeshift snapshot for the Ubuntu environment before significant changes.

## Bare-metal Windows extension

The Windows VM remains useful, but some games and anti-cheat systems may object to virtualization.

The current plan is to add a separate bare-metal Windows installation while retaining the Windows VM.

Planned workflow:

1. Start VM 100 and use it to prepare Windows installation media.
2. Shut down all guests before changing the disk layout.
3. Install Windows into its dedicated area.
4. Create or retain an NTFS game-library partition.
5. Never mount the shared NTFS volume from the Windows VM and bare-metal Windows simultaneously.
6. Use bare-metal Windows for incompatible games and the VM for ordinary Windows workloads.

This is an extension of the original design rather than a complete return to conventional dual booting. Ubuntu remains the primary workstation VM and Proxmox remains the core platform.

## Problems encountered

### Proxmox installer networking

The installer detected Ethernet and the AX200, but it did not offer a normal Wi-Fi SSID and authentication workflow.

**Resolution:** Bootstrap the host through ThinkPad Ethernet sharing and then install/configure the necessary Wi-Fi tooling.

### Incorrect temporary subnet

The Proxmox bridge initially remained configured for the normal household LAN while connected to the ThinkPad's `10.42.0.0/24` shared network.

**Resolution:** Temporarily change the bridge address and gateway to match the ThinkPad network.

### Keyboard layout reverted

The console keyboard layout returned to UK during setup even though the physical keyboard is US.

**Resolution:** Re-run Debian keyboard configuration and verify `/etc/default/keyboard`.

### GPU clearance

The preferred GPU slot order was physically impossible.

**Resolution:** Place the WX 3100 in the upper slot and RTX 4070 in the lower slot, then adjust PCIe configuration.

### Scope expansion

The project accumulated ideas including dashboards, a Raspberry Pi control plane, media hosting, AI workloads, and advanced segmentation.

**Resolution:** Keep V1 focused on the working dual-GPU workstation. Treat the current integration and recovery work as V1.5, and defer unrelated infrastructure.

## Completed milestones

- [x] Install Proxmox VE on bare metal
- [x] Enable SVM, IOMMU, and Above 4G Decoding
- [x] Install both GPUs in a workable physical layout
- [x] Configure the 4 TB NVMe VM storage
- [x] Create the Ubuntu workstation VM
- [x] Pass the WX 3100 through to Ubuntu
- [x] Create the Windows 11 gaming VM
- [x] Pass the RTX 4070 through to Windows
- [x] Configure OVMF and TPM 2.0
- [x] Configure Looking Glass B7 and KVMFR
- [x] Build a routed private VM network
- [x] Configure WireGuard management
- [x] Configure Synergy input sharing
- [x] Back up and restore the primary VMs
- [x] Restore internet access during the 2026-08-03 host setup
- [x] Confirm the Intel AX200 wireless adapter
- [x] Begin a dated public progress log

## Current next actions

- [ ] Finish the persistent AX200 Wi-Fi configuration
- [ ] Finalize keyboard and mouse capture/release behaviour
- [ ] Confirm all restored VM settings and PCI passthrough devices
- [ ] Create the Windows installation USB from VM 100
- [ ] Complete the bare-metal Windows storage work
- [ ] Test the NTFS shared-library safeguards
- [ ] Add sanitized configuration examples
- [ ] Update the architecture diagram
- [ ] Add screenshots and performance measurements
- [ ] Integrate the 2 × 2 TB HDD backup storage

## Public repository safety

The repository must not contain:

- Passwords
- Private keys
- WireGuard private keys
- VPN credentials
- API tokens
- Cookies
- Personally identifying paths or data
- Unnecessary public IP addresses

Private RFC1918 architecture examples may be documented, but they should not be treated as credentials or guaranteed permanent configuration.

## Overall assessment

V1 has achieved its central goal: the dual-GPU virtualized workstation architecture works.

V1.5 is now addressing the practical issues that determine whether the platform is reliable day to day:

- Recovery
- Routed Wi-Fi networking
- Input integration
- Looking Glass usability
- Storage changes
- Gaming compatibility
- Reproducible public documentation

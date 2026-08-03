# Roadmap

**Last updated:** 3 August 2026

This document tracks the development of the Proxmox Workstation Platform. The version labels describe practical milestones rather than fixed product releases.

## Version 1 — Core virtualized workstation

**Goal:** Prove that the architecture works and can be used as a real workstation.

### Proxmox host

- [x] Enable SVM and IOMMU in BIOS
- [x] Enable Above 4G Decoding
- [x] Install Proxmox VE on the 500 GB NVMe
- [x] Configure the 4 TB NVMe for primary VM storage
- [x] Detect and configure the Intel AX200 Wi-Fi adapter
- [x] Establish temporary internet access through a ThinkPad dock
- [x] Replace the initial Ethernet-only assumption with routed Wi-Fi networking
- [ ] Integrate the 2 × 2 TB HDDs into the long-term backup and bulk-storage design

### Ubuntu workstation VM

- [x] Create and configure the Ubuntu workstation VM
- [x] Install daily-use development and administration software
- [x] Configure AMD Radeon Pro WX 3100 passthrough
- [x] Confirm the VM can operate as the primary desktop
- [x] Configure QEMU Guest Agent and guest integration
- [x] Add WireGuard access between the VM and Proxmox host

### Windows gaming VM

- [x] Create the Windows 11 VM
- [x] Configure OVMF and TPM 2.0
- [x] Configure NVIDIA GeForce RTX 4070 passthrough
- [x] Install and test the Windows gaming environment
- [x] Add Looking Glass B7
- [ ] Complete broader performance and compatibility benchmarking

### Documentation

- [x] Create the initial architecture diagram
- [x] Create the initial Proxmox setup notes
- [x] Create the initial GPU passthrough notes
- [x] Create the initial lessons-learned document
- [x] Add a dated project progress log
- [ ] Add sanitized VM configuration examples
- [ ] Add screenshots and updated architecture diagrams

## Version 1.5 — Integration, recovery, and daily usability

**Goal:** Turn the working proof of concept into a reliable everyday platform.

### Display and input

- [x] Configure Looking Glass B7
- [x] Build and load the KVMFR DKMS module
- [x] Configure IVSHMEM for the Windows VM
- [x] Configure Synergy 3 between the Ubuntu VM and ThinkPad
- [ ] Finalize keyboard and mouse capture/release behaviour
- [ ] Refine automatic startup and shutdown workflows

### Networking

- [x] Use the Intel AX200 as the Proxmox Wi-Fi uplink
- [x] Create a private VM bridge
- [x] Route and NAT VM traffic through Wi-Fi
- [x] Add WireGuard host-to-workstation connectivity
- [x] Forward Synergy traffic to the Ubuntu VM
- [ ] Document a repeatable clean-install networking procedure

### Storage and recovery

- [x] Back up the principal VMs
- [x] Restore the Windows and Ubuntu VMs to the 4 TB NVMe storage
- [x] Verify that the restored disks are visible in Proxmox
- [x] Use snapshots before major guest changes
- [ ] Complete the revised storage layout
- [ ] Document recovery steps and restore validation
- [ ] Add a recurring backup policy

### Bare-metal Windows extension

Some games and anti-cheat systems may not be suitable for a Windows VM. V1.5 therefore includes a planned bare-metal Windows option alongside the virtualized workstation.

- [x] Preserve the Windows VM for normal Windows tasks
- [x] Restore VM storage before repartitioning work
- [ ] Create Windows installation media from VM 100
- [ ] Install bare-metal Windows in the planned storage area
- [ ] Create an NTFS game-library partition
- [ ] Ensure the shared NTFS volume is never mounted by two Windows instances simultaneously
- [ ] Test Proxmox and bare-metal Windows boot selection
- [ ] Document gaming compatibility differences

## Version 2 — Remote management and control plane

**Goal:** Enable secure remote power and management without making the workstation itself permanently active.

- [ ] Set up a Raspberry Pi with a minimal Linux installation
- [ ] Connect the Pi to the Proxmox Ethernet interface
- [ ] Connect the Pi to the household network over Wi-Fi
- [ ] Configure WireGuard or another suitable remote-access method
- [ ] Implement Wake-on-LAN for the Proxmox host
- [ ] Test the full remote workflow
- [ ] Document the control-plane architecture

## Version 3 — Monitoring and automation

- [ ] Deploy a lightweight dashboard
- [ ] Add Proxmox resource and VM-status monitoring
- [ ] Add scripts for common VM start, stop, and backup operations
- [ ] Add health checks for networking, storage, and GPU availability
- [ ] Explore safe automation of the daily workstation workflow

## Version 4 — Services and polish

- [ ] Evaluate Jellyfin or another media service
- [ ] Refine and automate the backup strategy
- [ ] Add network segmentation when suitable networking hardware is available
- [ ] Complete a full public documentation review
- [ ] Replace the original architecture diagram with the final design
- [ ] Publish a reproducible Proxmox and Looking Glass guide

## Future ideas

These are deliberately outside the current scope:

- Dedicated AI or compute VM using the RTX 4070
- Managed switch and VLAN segmentation
- NAS or expanded storage
- Custom tablet control dashboard
- Additional infrastructure when living arrangements allow it

The platform evolves according to what is genuinely useful rather than what merely sounds impressive.

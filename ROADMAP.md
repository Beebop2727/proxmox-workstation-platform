# Roadmap

**Last updated:** 14 August 2026

This document tracks development of the Proxmox Workstation Platform. Version labels describe practical milestones rather than fixed product releases.

## Version 1 — Core virtualized workstation

**Goal:** Prove the architecture works as a usable dual-GPU workstation.

### Proxmox host

- [x] Enable SVM and IOMMU
- [x] Enable Above 4G Decoding
- [x] Install Proxmox VE on the 500 GB NVMe
- [x] Configure the 4 TB NVMe for primary VM storage
- [x] Detect and configure the Intel AX200
- [x] Establish temporary internet access through a ThinkPad dock
- [x] Replace the Ethernet-only assumption with routed Wi-Fi networking
- [x] Establish persistent direct Wi-Fi operation on the Proxmox host
- [ ] Integrate the 2 × 2 TB HDDs into the long-term backup and bulk-storage design

### Ubuntu workstation VM

- [x] Create and configure the Ubuntu workstation VM
- [x] Install daily-use development and administration software
- [x] Configure AMD Radeon Pro WX 3100 passthrough
- [x] Confirm the VM can operate as the primary desktop
- [x] Configure QEMU Guest Agent and guest integration
- [x] Add management connectivity

### Windows environments

- [x] Create the Windows 11 VM
- [x] Configure OVMF and TPM 2.0
- [x] Configure NVIDIA GeForce RTX 4070 passthrough
- [x] Install and test the Windows gaming environment
- [x] Add Looking Glass B7
- [x] Install a separate bare-metal Windows 11 environment
- [x] Confirm bare-metal Windows / Proxmox boot selection works
- [ ] Complete broader performance and compatibility benchmarking
- [ ] Decide the long-term role of the Windows VM versus bare-metal Windows
- [ ] Document gaming compatibility differences

### Documentation

- [x] Create the initial architecture diagram
- [x] Create initial Proxmox setup notes
- [x] Create initial GPU passthrough notes
- [x] Create a lessons-learned document
- [x] Add a dated project progress log
- [x] Close and index the August infrastructure progress entries
- [ ] Add sanitized VM configuration examples
- [ ] Replace / extend the original architecture diagram with the current distributed design
- [ ] Add screenshots and performance measurements

## Version 1.5 — Integration, recovery, and daily usability

**Goal:** Turn the proof of concept into a reliable everyday platform.

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
- [x] Add WireGuard host management
- [x] Forward required workstation traffic through the routed design
- [x] Deploy an OPNsense firewall VM
- [x] Establish a dedicated OPNsense WireGuard management tunnel
- [x] Make the Proxmox-to-OPNsense WireGuard DNAT rule persistent
- [x] Configure OPNsense VM autostart
- [ ] Reserve or statically assign the OPNsense WAN address
- [ ] Tighten the OPNsense WireGuard management firewall rule
- [ ] Replace the temporary Wi-Fi uplink with Ethernet when practical
- [ ] Document a repeatable clean-install networking procedure

### Storage and recovery

- [x] Back up the principal VMs
- [x] Restore the Windows and Ubuntu VMs to the 4 TB NVMe
- [x] Verify restored disks are visible in Proxmox
- [x] Use snapshots before major guest changes
- [ ] Complete the revised storage layout
- [ ] Document recovery steps and restore validation
- [ ] Add a recurring backup policy

## Version 2 — Distributed infrastructure and control plane

**Goal:** Move supporting infrastructure away from the main hypervisor and provide secure management foundations.

The earlier Raspberry Pi control-plane idea has been superseded by repurposing an existing MacBook as an Ubuntu Server infrastructure host.

### Infrastructure host

- [x] Repurpose the MacBook as a separate infrastructure node
- [x] Replace macOS with Ubuntu Server
- [x] Restore working Wi-Fi on the MacBook
- [x] Establish SSH administration
- [x] Add WireGuard connectivity
- [x] Deploy Unbound for lab-oriented DNS
- [x] Configure secure upstream DNS
- [ ] Add infrastructure monitoring
- [ ] Implement Wake-on-LAN / local power-control workflows
- [ ] Decide which additional supporting services belong on the infrastructure host

### Firewall and segmentation

- [x] Deploy OPNsense on Proxmox
- [x] Configure WAN and LAN interfaces
- [x] Configure LAN DHCP
- [x] Establish firewall management access
- [x] Establish dedicated WireGuard firewall management
- [ ] Introduce VLAN segmentation when suitable networking hardware is available
- [ ] Create separate trusted, server, lab/security, and management segments
- [ ] Review DNS policy across future VLANs

### Remote access

- [x] Establish local WireGuard management foundations
- [ ] Evaluate secure remote access from outside the local network
- [ ] Avoid exposing hypervisor or firewall administration directly to the Internet
- [ ] Test a complete remote wake / management workflow if remote access is adopted

## Version 3 — Monitoring and automation

- [ ] Deploy a lightweight infrastructure dashboard
- [ ] Add Proxmox resource and VM-status monitoring
- [ ] Add OPNsense / network-health monitoring
- [ ] Add scripts for common VM start, stop, and backup operations
- [ ] Add health checks for networking, storage, and GPU availability
- [ ] Explore safe automation of the daily workstation workflow

## Version 4 — Services and polish

- [ ] Evaluate Jellyfin or another media service
- [ ] Refine and automate the backup strategy
- [ ] Complete a full public documentation review
- [ ] Replace the original architecture diagram with the final design
- [ ] Publish a reproducible Proxmox / Looking Glass guide
- [ ] Publish a sanitized firewall / segmented-lab design

## Future ideas

These remain intentionally optional:

- Dedicated AI or compute VM using the RTX 4070
- Managed switch and VLAN expansion
- NAS or expanded storage
- Custom tablet control dashboard
- Additional infrastructure services
- Additional WireGuard peers where genuinely useful

The platform evolves according to what is practically useful rather than simply adding complexity.

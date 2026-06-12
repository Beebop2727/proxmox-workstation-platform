# Roadmap

This document outlines the planned development of the Proxmox Workstation Platform across multiple versions. Each version builds on the last, adding capability without disrupting the working foundation beneath it.

---

## Version 1 — Core virtualized workstation

The goal of Version 1 is simple: prove the architecture works and use it daily.

**Proxmox host**
- [ ] Enable SVM and IOMMU in BIOS
- [ ] Install Proxmox VE on 500 GB NVMe
- [ ] Configure storage pools (4 TB NVMe for VMs, 2× 2 TB HDD for backups)
- [ ] Set up network bridge for VM connectivity

**Ubuntu workstation VM**
- [ ] Create and configure Ubuntu VM
- [ ] Install daily-use software (VS Code, Git, browser, ProtonVPN)
- [ ] Confirm stable workflow as primary desktop environment
- [ ] Configure AMD Radeon Pro WX 3100 passthrough

**Windows gaming VM**
- [ ] Create Windows VM
- [ ] Configure NVIDIA RTX 4070 passthrough
- [ ] Install Steam and test game library
- [ ] Verify gaming performance

**Documentation**
- [ ] Architecture diagram
- [ ] Proxmox setup notes
- [ ] GPU passthrough notes
- [ ] Lessons learned

---

## Version 2 — Remote management and control plane

Version 2 introduces the Raspberry Pi as an always-on control plane, enabling the desktop to be managed and powered remotely.

- [ ] Set up Raspberry Pi with a base Linux install
- [ ] Configure Tailscale on the Pi and both client devices (ThinkPad, MatePad Pro)
- [ ] Implement Wake-on-LAN from the Pi to the Proxmox host
- [ ] Test full remote workflow: wake desktop → connect to VM → work → shutdown
- [ ] Document control plane architecture

---

## Version 3 — Monitoring and automation

Version 3 adds visibility and automation on top of the working infrastructure.

- [ ] Deploy a lightweight dashboard (e.g. Homepage or Dashy)
- [ ] Add Proxmox monitoring (resource usage, VM status)
- [ ] Write automation scripts for common tasks (VM start/stop, backups)
- [ ] Explore Looking Glass for low-latency Windows VM access from the Ubuntu desktop

---

## Version 4 — Services and polish

Version 4 rounds out the platform with self-hosted services and project polish.

- [ ] Set up Jellyfin for media hosting on the 2× 2 TB HDDs
- [ ] Refine backup strategy and automate VM snapshots
- [ ] Add network segmentation if router/switch situation changes (e.g. own flat)
- [ ] Full GitHub documentation review and cleanup
- [ ] Architecture diagram updated to reflect final state

---

## Future ideas

Things worth exploring further down the line, intentionally excluded from the current scope:

- Dedicated AI/compute VM leveraging the RTX 4070
- Managed switch and VLAN segmentation
- NAS or expanded storage
- Proper rack infrastructure (own place, longer term)

---

*Versions 2–4 and future ideas are subject to change. The project evolves based on what's actually useful, not what sounds impressive on paper.*

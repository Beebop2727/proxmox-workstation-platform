# Proxmox Workstation RAM Upgrade Decision

**Decision date:** 17 August 2026  
**System:** Proxmox workstation / gaming PC  

## Decision

Upgrade the system from **32 GB (4 × 8 GB)** to **64 GB (2 × 32 GB)** using:

- **Manufacturer:** SK Hynix
- **Model:** HMAA4GU6CJR8N-XN
- **Capacity:** 32 GB per DIMM
- **Total:** 64 GB
- **Memory type:** DDR4 UDIMM
- **Speed:** DDR4-3200 / PC4-25600
- **Timings:** CL22
- **Nominal voltage:** 1.2 V
- **Configuration:** 2 × 32 GB
- **Price observed:** £89.99 per DIMM
- **Estimated total:** £179.98
(RAM is extremely expensive as of this date 2026-08-17, even for DDR4)

The modules are standard unbuffered desktop DDR4 rather than registered server RAM, making them appropriate for the X570 platform. The modules are significantly cheaper than those catered towards "Gamers", aving budget of the build that could be placed elsewhere in the system.

## Why This Upgrade

The current 32 GB configuration is increasingly restrictive for the Proxmox-based cybersecurity lab.

64 GB provides substantially more capacity for:

- Windows test VMs
- Ubuntu workstation VM
- Parrot/Kali lab machines
- OPNsense
- Monitoring and logging services
- Additional Windows/Linux servers
- Disposable security-testing environments

The upgrade prioritises **capacity, stability and cost** over low-latency gaming-oriented memory.

## Gaming Impact

The Hynix modules use JEDEC DDR4-3200 CL22 rather than tighter gaming-oriented timings such as 3200 CL16.

A small reduction in CPU-limited gaming performance is acceptable.

Games currently runs with significant performance headroom, so preserving maximum memory latency performance is not a priority compared with increasing available RAM for the lab.

## Stability Testing

After installation:

1. Confirm all 64 GB is visible in BIOS.
2. Boot Proxmox.
3. Confirm the host reports the expected memory capacity.
4. Run a memory test.
5. Stress the system with multiple VMs.
6. Check for crashes, memory errors, VM instability or unexpected reboots.

Only after the 64 GB configuration is proven stable should further memory tuning be considered.

## Optional Future Experiment: 80 GB

The existing **4 × 8 GB gaming RAM** can be retained for testing.

A future configuration could be:

- 2 × 32 GB Hynix
- 2 × 8 GB existing gaming RAM
- **80 GB total**

This mixed configuration is not part of the initial upgrade decision.

If tested, the memory may need to run at approximately **2933–3000 MT/s** with conservative timings and a common voltage supported by all four DIMMs.

The 64 GB Hynix-only configuration remains the preferred baseline because it should provide better compatibility and place less stress on the Ryzen 9 3900X memory controller.

## Final Decision

Proceed with **2 × 32 GB SK Hynix HMAA4GU6CJR8N-XN DDR4-3200 UDIMMs** for a **64 GB baseline configuration**.

The target is a stable DDR4-3200 setup, with **3000 MT/s considered fully acceptable** if required.

The existing 4 × 8 GB kit will be kept aside rather than mixed immediately, allowing the new 64 GB configuration to be validated independently before experimenting with an 80 GB setup.

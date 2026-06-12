# GPU passthrough notes

This document records the GPU passthrough configuration for both VMs. Passthrough is the most complex part of this build — notes are kept here in detail for future reference and troubleshooting.

---

## Hardware

| GPU | Assigned to | Slot |
|---|---|---|
| AMD Radeon Pro WX 3100 | Ubuntu workstation VM | PCIe slot 2 |
| NVIDIA RTX 4070 | Windows gaming VM | PCIe slot 1 (primary) |

---

## IOMMU groups

IOMMU grouping determines which devices can be passed through together. Both GPUs need to be in separate IOMMU groups from each other and from devices you want to keep on the host.

```bash
# Run this to list IOMMU groups after enabling IOMMU in BIOS
for d in /sys/kernel/iommu_groups/*/devices/*; do
  n=${d#*/iommu_groups/*}; n=${n%%/*}
  printf 'IOMMU Group %s ' "$n"
  lspci -nns "${d##*/}"
done
```

<!-- Paste output here -->

---

## Kernel parameters

Added to `/etc/default/grub` on the Proxmox host:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt"
```

<!-- Note any additional parameters needed -->

---

## VFIO modules

Added to `/etc/modules`:

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

---

## GPU IDs

```bash
# Run to get PCI IDs for passthrough devices
lspci -nn | grep -i vga
```

| Device | PCI ID |
|---|---|
| AMD Radeon Pro WX 3100 | <!-- e.g. 1002:687f --> |
| NVIDIA RTX 4070 | <!-- e.g. 10de:2786 --> |

Added to `/etc/modprobe.d/vfio.conf`:

```
options vfio-pci ids=XXXX:XXXX,XXXX:XXXX
```

---

## Ubuntu VM passthrough config

Proxmox VM settings:

| Setting | Value |
|---|---|
| Machine type | q35 |
| BIOS | OVMF (UEFI) |
| PCIe device | AMD Radeon Pro WX 3100 |
| ROM-Bar | Enabled |
| Primary GPU | Yes |

<!-- Note any issues encountered -->

---

## Windows VM passthrough config

Proxmox VM settings:

| Setting | Value |
|---|---|
| Machine type | q35 |
| BIOS | OVMF (UEFI) |
| PCIe device | NVIDIA RTX 4070 |
| ROM-Bar | Enabled |
| Primary GPU | Yes |

### NVIDIA driver notes

<!-- Windows VM may require hiding hypervisor from NVIDIA drivers -->
<!-- Add CPU flags if needed: -cpu host,kvm=off,hv_vendor_id=proxmox -->

---

## Issues encountered

<!-- Document problems and solutions here as you go — this section often ends up being the most useful -->

| Issue | Cause | Fix |
|---|---|---|
| | | |

---

## References

- [Proxmox PCI Passthrough wiki](https://pve.proxmox.com/wiki/PCI_Passthrough)
- [Proxmox VFIO guide](https://pve.proxmox.com/wiki/PCI(e)_Passthrough)

# Decision Records

> [!NOTE]
> These records document major architectural and technical decisions made during development of the Proxmox Workstation Platform.
>
> The project was shelved on **4 September 2026**. Earlier decisions remain in this repository as historical context even where they were later superseded.

## Decisions

| Date                                                       | Decision                                         | Outcome                                                                                           |
| ---------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| [5 August 2026](2026-08-05-windows-environments.md)        | Windows environment strategy                     | Windows VM retained alongside bare-metal Windows                                                  |
| [10 August 2026](2026-08-10-distributed-infrastructure.md) | Distributed infrastructure                       | Supporting services moved away from the main Proxmox host                                         |
| [17 August 2026](2026-08-17-ram-upgrade-decision.md)       | RAM upgrade                                      | Workstation upgraded to 64 GB                                                                     |
| [25 August 2026](2026-08-25-hyprland-decision.md)          | Switch Ubuntu workstation from GNOME to Hyprland | Approved, later superseded by bare-metal Ryoku migration                                          |
| [27 August 2026](2026-08-27-obsidian-brain.md)             | Documentation knowledge-base design              | Markdown-based research and decision documentation adopted                                        |
| **4 September 2026**                                       | **Shelve Proxmox Workstation Platform**          | **Proxmox removed from the workstation architecture; successor design moves Linux to bare metal** |

## Supersession

Decision records represent the reasoning available at the time they were written.

A later decision may supersede an earlier one without making the earlier decision invalid historically.

The most significant example is the planned Hyprland migration.

The original plan was:

```text
Proxmox
   |
Ubuntu workstation VM
   |
Hyprland
```

The successor architecture instead moves toward:

```text
Ryoku Linux / Hyprland
        |
   KVM / QEMU
        |
       VMs
```

This keeps virtualisation available without requiring the primary desktop itself to run inside a VM.

## Project closure

The final architectural decision is to preserve this repository as a historical record rather than continue adapting the Proxmox design.

Useful concepts including GPU passthrough, VFIO, IOMMU, disposable VMs and isolated lab environments are expected to continue in the successor project.

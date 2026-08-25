# Proxmox Workstation — Hyprland Migration Plan

## Overview

The Proxmox workstation currently uses an Ubuntu workstation VM as the primary Linux desktop environment.

The next planned desktop change is to move the Ubuntu workstation away from its current GNOME-based workflow and begin using **Hyprland** as the primary Wayland compositor.

This is intended to make the desktop lighter, more responsive, and more customisable while keeping the existing Proxmox virtualisation setup unchanged.

## Current Environment

* **Hypervisor:** Proxmox VE
* **Primary Linux VM:** Ubuntu Workstation
* **GPU passthrough:** AMD Radeon Pro WX 3100
* **Windows VM GPU:** NVIDIA GeForce RTX 4070
* **Remote / shared input:** Synergy
* **Host management:** WireGuard
* **Current Linux desktop:** GNOME
* **Planned desktop:** Hyprland

## Planned Changes

The Hyprland migration will focus only on the Ubuntu workstation VM.

Planned work includes:

* Install and configure Hyprland alongside the existing desktop.
* Keep GNOME available initially as a fallback session.
* Configure monitor layout, scaling, and refresh-rate behaviour.
* Configure keyboard, mouse, touchpad, and input sensitivity.
* Build and refine the desktop shell and panel configuration.
* Configure application launcher, notifications, clipboard tools, screenshots, and lock screen.
* Configure PipeWire audio integration.
* Confirm AMD GPU acceleration and Wayland support.
* Test Synergy and other cross-system input workflows.
* Recreate essential shortcuts used in the existing Ubuntu setup.
* Move desktop customisations into version-controlled configuration files.

## Migration Strategy

1. Build and test the Hyprland configuration on the ThinkPad.
2. Use a forked version of **Vast Shell** as the base configuration.
3. Customise the fork to suit the intended Proxmox workstation workflow.
4. Test stability, layout, shortcuts, input behaviour, and desktop components on the ThinkPad.
5. Install Hyprland alongside the existing GNOME session on the Ubuntu workstation VM.
6. Transfer the tested Vast Shell fork and related configuration files to the Ubuntu VM.
7. Confirm that both GNOME and Hyprland sessions launch correctly.
8. Test GPU acceleration and display behaviour with the AMD Radeon Pro WX 3100.
9. Test daily applications and Proxmox-related workflows.
10. Resolve any remaining compatibility or stability problems.
11. Use Hyprland as the default session once it is considered stable.
12. Keep GNOME temporarily as a recovery option.

## Testing Checklist

* [ ] Hyprland launches reliably.
* [ ] Vast Shell fork loads correctly.
* [ ] Custom shell changes behave as expected.
* [ ] AMD Radeon Pro WX 3100 acceleration works correctly.
* [ ] Display resolution and scaling are correct.
* [ ] Adaptive / variable refresh behaviour works as expected.
* [ ] Keyboard and mouse configuration is correct.
* [ ] Audio works through PipeWire.
* [ ] Browser hardware acceleration works.
* [ ] Synergy works correctly between systems.
* [ ] Clipboard tools work.
* [ ] Screenshot tools work.
* [ ] Notifications work.
* [ ] Screen locking works.
* [ ] Suspend / resume behaviour is stable.
* [ ] Essential applications behave correctly under Wayland.
* [ ] Configuration files are backed up and committed to Git.
* [ ] ThinkPad configuration can be transferred cleanly to the Proxmox Ubuntu VM.

## Scope

This migration does **not** change the underlying Proxmox architecture.

The following will remain in place:

* Existing VM layout
* GPU passthrough configuration
* OPNsense networking
* WireGuard management
* Windows 11 VM
* Parrot OS lab VMs
* Existing Proxmox storage configuration

Hyprland is therefore a desktop-environment change within the Ubuntu workstation VM rather than a redesign of the virtualisation platform.

## Current Development

A dedicated Hyprland configuration is currently being built and tested on the **ThinkPad** using a **forked version of Vast Shell** as the foundation.

The fork is being customised to match the intended Proxmox workstation workflow rather than using the upstream Vast Shell configuration unchanged.

The ThinkPad is being used as the development and validation system for the fork before the finished setup is transferred to the Proxmox Ubuntu workstation VM.

This allows the desktop layout, keybindings, shell behaviour, application launcher, notifications, input settings, visual design, and general workflow to be refined without disrupting the primary Proxmox workstation.

Once the ThinkPad configuration is considered stable, the fork and related Hyprland configuration will be migrated to the Ubuntu workstation VM for GPU passthrough and full workstation testing.

## Expected Outcome

The goal is to create a more streamlined and highly customised Linux workstation experience while preserving the flexibility of the existing Proxmox environment.

The completed setup should provide:

* A lightweight Wayland-based desktop.
* A consistent custom interface based on the Vast Shell fork.
* Faster access to frequently used applications and system functions.
* Improved keyboard-driven workflows.
* Better control over display and workspace behaviour.
* Version-controlled desktop configuration.
* A reusable Hyprland setup that can be maintained and improved over time.

## Status

**Status:** 🚧 Planned / In Progress

The Hyprland configuration is actively being developed on the ThinkPad using a forked version of Vast Shell.

Once the configuration is considered stable, it will be transferred to the Proxmox Ubuntu workstation VM and tested with the existing AMD GPU passthrough, networking, audio, and desktop workflow.

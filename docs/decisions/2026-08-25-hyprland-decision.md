# Decision Record — Switch to Hyprland

## Decision

The Ubuntu workstation environment will transition from GNOME to **Hyprland** as the primary desktop compositor.

## Reasoning

The main reasons for the change are:

* **Greater customisation:** Hyprland provides more control over layout, keybindings, workspaces, animations, panels, and overall desktop behaviour.
* **Improved stability:** The existing GNOME setup has experienced crashes and instability linked to additional GNOME extensions.
* **Reduced dependency on extensions:** Hyprland allows much of the desired workflow to be configured directly rather than relying on multiple third-party desktop extensions.
* **Better fit for the workstation:** A custom Hyprland environment can be tailored specifically around the Proxmox workstation workflow.

## Implementation

A custom Hyprland configuration is currently being developed and tested on the **ThinkPad** using a forked version of **Vast Shell**.

Once stable, the configuration will be transferred to the Ubuntu workstation VM and tested with the existing GPU passthrough and workstation setup.

## Status

**Decision:** ✅ Approved
**Implementation:** 🚧 In progress
::: 

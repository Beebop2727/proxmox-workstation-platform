# OPNsense Firewall & WireGuard Management Progress

**Date:** 14 August 2026  
**Status:** 🟢 Operational, with minor follow-up work remaining

---

## What Was Operational at the Start

At the start of the session:

- OPNsense was already installed as a Proxmox VM.
- The VM had separate WAN and LAN interfaces.
- OPNsense WAN was reachable from the Proxmox host.
- The OPNsense web interface was available through the existing management path.
- A dedicated WireGuard instance had been created on OPNsense.
- The trusted client had a WireGuard peer configuration, but the tunnel was not yet fully usable for management.
- Proxmox already had its own separate WireGuard management tunnel, which was intentionally left unchanged.

The goal for this session was to complete the OPNsense management tunnel, make the required forwarding persistent, and confirm the firewall could be managed directly through WireGuard.

---

# Work Completed

## 1. Completed the WireGuard Management Path

A dedicated WireGuard management network was completed between a trusted client and OPNsense.

Current tunnel addressing:

```text
OPNsense WireGuard:  10.255.99.1/24
Trusted client:      10.255.99.2/24
WireGuard UDP port:  51821
```

The client routes the following networks through this tunnel:

```text
10.255.99.0/24
10.10.10.0/24
```

A successful handshake was confirmed with non-zero transmitted and received traffic.

---

## 2. Confirmed the Full Packet Path

The current connection path is:

```text
Trusted Client
    |
    | UDP 51821
    v
Proxmox upstream interface
    |
    | DNAT
    v
OPNsense WAN
    |
    | WireGuard
    v
10.255.99.1
```

Packet capture was used to verify each stage:

1. The client generated WireGuard initiation packets.
2. Proxmox received the packets on its upstream interface.
3. The packets were destination-NATed toward the OPNsense WAN address.
4. OPNsense received the packets on its WAN interface.
5. WireGuard completed a handshake.
6. Traffic passed successfully through the encrypted tunnel.

This confirmed that the original issue was not related to WireGuard keys or routing on the client.

---

## 3. Added the Correct OPNsense WAN Rule

The WireGuard listener is reached through the OPNsense WAN interface, so a dedicated inbound WAN rule was added.

```text
Action:       Pass
Interface:    WAN
Direction:    in
Protocol:     UDP
Source:       Trusted client upstream address
Destination:  WAN address
Port:         51821
```

An earlier rule had been placed on LAN, but packet capture confirmed that WireGuard initiation traffic entered OPNsense through WAN instead.

The unnecessary LAN WireGuard listener rule can therefore be removed.

---

## 4. Added WireGuard Management Firewall Access

A rule was added to the OPNsense WireGuard firewall group so that the trusted tunnel client can manage the firewall itself.

Current rule:

```text
Interface:    WireGuard
Source:       10.255.99.2/32
Destination:  This Firewall
Protocol:     Any
```

This allowed:

```text
ping 10.255.99.1
```

and direct access to the OPNsense web interface at:

```text
https://10.255.99.1
```

The rule is intentionally limited to a single WireGuard peer and the firewall itself.

It can later be tightened to only the required protocols, such as HTTPS and ICMP.

---

## 5. Preserved a Fallback Management Path

A separate WAN rule remains in place allowing the Proxmox host to access the OPNsense HTTPS interface.

This is useful as a recovery path if the WireGuard management configuration is changed or temporarily unavailable.

Conceptually:

```text
Proxmox host
    |
    | HTTPS
    v
OPNsense WAN
```

This fallback proved useful during configuration and is being retained for now.

---

# Reboot / Conntrack Issue

## Problem

After rebooting Proxmox:

- The DNAT service had started successfully.
- The DNAT rule existed.
- The client was sending UDP packets to the correct destination.
- OPNsense itself was running and reachable from Proxmox.
- However, a fresh WireGuard handshake did not occur.

The NAT rule initially showed no new matching connection.

## Cause

The WireGuard client had started sending UDP traffic before the DNAT rule became effective.

The existing UDP flow was already present in connection tracking, so subsequent packets continued using the earlier non-NAT state.

Restarting WireGuard on the client changed its UDP source port and immediately created a fresh, correctly translated flow.

## Fix

The systemd service was updated with a targeted conntrack cleanup:

```bash
conntrack -D -p udp --dst <PROXMOX_UPSTREAM_IP> --dport 51821
```

This removes only the tracked WireGuard UDP flow after the DNAT rule is installed.

The next packet can then create a new NAT-aware connection automatically.

---

# Problems Encountered

## Firewall Rule Placement

The first WireGuard listener rule was created on LAN.

Packet capture later confirmed the encrypted packets actually arrive on OPNsense WAN.

The correct rule was therefore added to WAN.

---

## Temporary Loss of GUI Access

During firewall configuration, the existing WAN HTTPS management rule was accidentally replaced while creating the WireGuard listener rule.

The active browser connection initially remained open because of existing firewall state, but later dropped when the firewall was reloaded.

The HTTPS management rule was restored before continuing.

---

## WireGuard Traffic Allowed on WAN but Blocked Inside the Tunnel

After the WireGuard handshake was working, traffic to `10.255.99.1` still failed.

The solution was to add a firewall rule to the automatically created WireGuard group.

After applying that rule:

```text
10.255.99.2 -> 10.255.99.1
```

was reachable and the OPNsense GUI loaded successfully through the tunnel.

---

## USB Passthrough Conflict

During reboot testing, a physical USB keyboard was found to be attached to more than one VM, causing repeated detach/attach messages.

This was unrelated to OPNsense networking but created significant console noise.

The keyboard should only be passed through to the VM that actually requires it.

---

## Wi-Fi Packet Loss

The Proxmox host currently uses Wi-Fi rather than Ethernet for its upstream connection.

Testing from the management client showed the local gateway was stable, but communication directly with the Proxmox host experienced intermittent packet loss and latency spikes.

The OPNsense/WireGuard setup itself remained functional.

This is currently accepted as a limitation of the temporary Wi-Fi-based network arrangement.

---

# Decisions & Trade-offs

## Keep Proxmox's Existing WireGuard Separate

The existing Proxmox WireGuard tunnel and the new OPNsense WireGuard tunnel are intentionally separate.

```text
Existing Proxmox management tunnel
    -> remains unchanged

OPNsense management tunnel
    -> UDP 51821
    -> dedicated 10.255.99.0/24 network
```

This avoids coupling the new firewall deployment to an already working management tunnel.

---

## Keep a Direct Proxmox Recovery Path

Although OPNsense is now manageable through WireGuard, direct HTTPS access from Proxmox to the OPNsense WAN interface is being retained as a fallback.

This makes future firewall experimentation less likely to leave the firewall completely inaccessible.

---

## Accept Wi-Fi Temporarily

The present design is not intended to treat Wi-Fi as the permanent upstream network for the firewall platform.

It is currently being accepted so development can continue.

A wired uplink is preferred later.

---

## Resource Usage

The OPNsense VM was initially allocated 4 GB of RAM.

Because the host has multiple workstation and lab VMs, memory usage will be reviewed and potentially reduced after observing real-world OPNsense utilisation.

The objective is to keep enough memory available for larger Windows and Linux workstation VMs while maintaining a stable firewall.

---

# Current State

At the end of the session:

```text
OPNsense VM:             Operational
OPNsense autostart:      Enabled

WAN:                     10.10.10.144/24
LAN:                     10.10.99.1/24

WireGuard network:       10.255.99.0/24
OPNsense WG address:     10.255.99.1
Trusted client:          10.255.99.2
WireGuard port:          UDP 51821

WireGuard handshake:     Working
Tunnel traffic:          Working
OPNsense GUI over WG:    Working

Proxmox DNAT:            Working
DNAT persistence:        Configured
Conntrack recovery:      Configured

Fallback management:     Available
```

The core OPNsense management deployment is now considered operational.

---

# Next Actions

- Perform one final cold-boot test without manually restarting WireGuard on the client.
- Confirm the conntrack cleanup restores the management tunnel automatically after boot.
- Reserve or statically assign the OPNsense WAN address so the DNAT target cannot change.
- Tighten the WireGuard management firewall rule from `any` to required services only.
- Remove the redundant LAN WireGuard listener rule if still present.
- Review OPNsense RAM usage and reduce allocation if practical.
- Replace the temporary Wi-Fi upstream connection with Ethernet when available.
- Continue toward VLAN-based network segmentation.
- Add further trusted WireGuard peers only when required.

---

## Security Notes

This public progress log intentionally excludes:

- WireGuard private keys.
- WireGuard public keys.
- Authentication credentials.
- MAC addresses.
- Usernames.
- Deployment-specific upstream LAN addresses.
- Any secrets or tokens.

The values included are private lab addressing and general configuration details only.

# Lessons learned

A running log of decisions, mistakes, and things worth remembering from the build process.

## Proxmox installation and networking

- A normal Wi-Fi station interface cannot generally be attached to a Linux bridge in the same way as Ethernet. Routed/NAT networking is the practical design for the current location.
- Temporary connectivity through another Linux machine is useful for bootstrapping a hypervisor when the final network path is not available yet.
- Hypervisor networking should be treated as infrastructure: document interface roles, subnets, and recovery paths before making firewall changes.
- Wi-Fi can work for development, but it introduces latency and packet-loss behaviour that makes firewall troubleshooting harder. Ethernet remains the preferred long-term uplink.

## GPU passthrough

- Physical slot placement can be constrained by cooler size even when the logical PCIe plan looks valid.
- IOMMU, Above 4G Decoding, OVMF, and correct device isolation matter more than trying to optimize the build prematurely.
- A working passthrough configuration should be backed up before experimenting with display, IVSHMEM, or input changes.

## Windows environments

- A Windows gaming VM is useful even when bare-metal Windows exists: snapshots, testing, maintenance, and compatible applications still benefit from virtualization.
- Anti-cheat compatibility is a legitimate reason to retain a native Windows option.
- Do not force one environment to solve every workload when two clearly separated environments are more reliable.

## Infrastructure host

- Fighting host-level network filtering can cost more time than replacing the host OS when the machine's role is infrastructure rather than desktop use.
- The MacBook migration from macOS to Ubuntu Server simplified SSH, WireGuard, DNS, and future service deployment.
- Reusing existing hardware can replace planned purchases such as a dedicated Raspberry Pi control plane.

## OPNsense and WireGuard

- Always identify the interface on which packets actually enter the firewall before creating rules. Packet capture is more reliable than assumptions.
- A WireGuard handshake proves encrypted transport, but not necessarily that traffic inside the tunnel is permitted. OPNsense still needs rules on the WireGuard interface/group.
- Keep a separate recovery management path while changing firewall rules.
- NAT persistence is not just about restoring an iptables rule: connection tracking can preserve an earlier non-NAT UDP flow across the time when the rule is installed.
- A targeted `conntrack -D` for the WireGuard destination is safer than flushing the entire connection-tracking table.
- Do not store WireGuard private keys in the public repository.

## Documentation

- Dated progress logs are useful because architecture decisions change rapidly.
- Historical entries should remain historical; later entries should explicitly close or supersede earlier open items.
- Indexes and roadmaps need maintenance too, otherwise the repository can look significantly less complete than the actual platform.

## Things to improve next

- Move the Proxmox uplink to Ethernet when practical.
- Reserve or statically assign infrastructure addresses used by persistent forwarding rules.
- Introduce VLAN segmentation only after the physical network is ready.
- Build a real recurring backup policy rather than relying only on ad-hoc snapshots and manual backups.

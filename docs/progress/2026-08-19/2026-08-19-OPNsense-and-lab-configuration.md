# Proxmox Homelab Progress — 19 August 2026

## Summary

Today focused on improving the Proxmox lab network, stabilising OPNsense, setting up easier terminal access, and starting a lightweight monitoring dashboard.

## Completed

### OPNsense / Lab Network

- OPNsense VM: **VM 104**
- WAN: `10.10.10.144/24` on `vmbr0`
- LAN: `10.10.99.1/24` on `vmbr1`
- Lab subnet: `10.10.99.0/24`
- Parrot disposable VM moved to the OPNsense LAN.
- Parrot address: `10.10.99.150`
- Internet access through OPNsense confirmed.
- DNS through OPNsense confirmed.
- Lab firewall policy confirmed:
  - Allow DNS to OPNsense.
  - Block access from the lab to RFC1918/private networks.
  - Allow Internet access.
- ThinkPad WireGuard access to the lab subnet is working.
- ThinkPad can directly manage the lab while lab systems cannot initiate connections back into trusted private networks.

### OPNsense Memory

OPNsense was becoming unresponsive while Suricata was being tested.

Current Proxmox memory configuration:

- Maximum memory: **8 GB**
- Balloon/minimum target: **6 GB**

Suricata is parked for now rather than spending more time tuning it today.

### Suricata

The rule downloader was successfully fixed and populated.

Current rule directory:

`/usr/local/etc/suricata/rules/`

Downloaded rule files include:

- abuse.ch Feodo Tracker
- abuse.ch SSL Blacklist
- abuse.ch ThreatFox
- abuse.ch URLHaus
- BotCC
- Compromised hosts
- DROP
- Emerging Threats attack response
- Coinminer
- Current events
- DNS
- Exploit
- Exploit Kit
- Hunting
- Malware
- Phishing
- Scan

Total `.rules` files: **18**

Suricata tuning/testing is intentionally postponed.

### Terminal Management

Installed and configured **Tabby** as the main terminal application.

Created/planned dedicated SSH profiles for:

- Proxmox
- OPNsense
- Ubuntu workstation
- Parrot
- mac-server
- Local ThinkPad

tmux was tested for persistent sessions, but it added unwanted mouse/right-click behaviour. The preferred everyday workflow is now **Tabby + normal SSH**, with tmux available only when a genuinely persistent remote session is useful.

Parrot SSH server was installed/enabled for remote access.

### Future Privacy Gateway Idea

Parked for a later revision:

- Dedicated isolated privacy bridge, e.g. `vmbr2`
- Tiny VPN gateway VM
- Self-hosted WireGuard VPN server on a cheap VPS outside the UK
- Potential locations: Netherlands, Finland, Switzerland
- Fail-closed design: VPN down = no Internet
- DNS forced through VPN
- Explicit IPv4/IPv6 leak protection
- Build routing, NAT and firewall rules manually as a learning project

### Future Home Assistant / Voice Project

Also parked:

- Home Assistant OS on Proxmox
- OpenAI conversation agent
- Reuse existing 2nd-generation Amazon Echo/Alexa as the voice interface
- Eventually expose selected homelab controls/status:
  - Proxmox VM status
  - VM start/stop
  - IDS alerts
  - Home sensors/devices
- Keep AI permissions tightly scoped.

## Uptime Kuma

Started deployment of a lightweight Uptime Kuma monitoring dashboard.

### LXC

- CT ID: **105**
- Hostname: `uptime-kuma`
- Debian 13
- IP: `10.10.10.105/24`
- Gateway: `10.10.10.1`
- Bridge: `vmbr0`
- 1 vCPU
- 1 GB RAM
- 512 MB swap
- 8 GB disk on `local-lvm`
- Unprivileged container
- Start at boot enabled

### Network Tests

From CT 105:

- `10.10.10.1` reachable
- `1.1.1.1` reachable
- `google.com` reachable
- DNS working
- Internet working

### Next Uptime Kuma Steps

Inside CT 105:

```bash
apt update
apt install -y nodejs npm git ca-certificates curl
```

Check versions:

```bash
node --version
npm --version
git --version
```

If Node.js is sufficiently current, continue:

```bash
cd /opt
git clone https://github.com/louislam/uptime-kuma.git
cd uptime-kuma
npm run setup
```

Then install/run with PM2 and access:

`http://10.10.10.105:3001`

Planned monitors:

- Proxmox
- OPNsense
- Ubuntu VM
- Parrot
- mac-server
- WireGuard
- Internet/DNS

## Current Key Addresses

| Device / Service | Address |
|---|---|
| Proxmox management | `192.168.1.94` |
| Proxmox vmbr0 | `10.10.10.1` |
| OPNsense WAN | `10.10.10.144` |
| OPNsense LAN | `10.10.99.1` |
| Ubuntu workstation VM | `10.10.10.132` |
| Parrot disposable VM | `10.10.99.150` |
| Uptime Kuma LXC | `10.10.10.105` |
| ThinkPad WireGuard | `10.255.99.2` |
| mac-server WireGuard | `172.31.255.3` |

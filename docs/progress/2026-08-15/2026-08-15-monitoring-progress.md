# Progress Update — 15 August 2026

**Focus:** MacBook infrastructure monitoring  
**Status:** 🟢 Monitoring foundation operational

## Completed

The Ubuntu Server MacBook was extended into a central monitoring node for the homelab.

- Installed and enabled **Grafana**
- Confirmed Grafana is reachable across the LAN from both the ThinkPad and Huawei MatePad
- Installed and enabled **Prometheus**
- Installed and enabled **Prometheus Node Exporter**
- Restricted Prometheus and Node Exporter to localhost only:
  - Prometheus: `127.0.0.1:9090`
  - Node Exporter: `127.0.0.1:9100`
- Kept Grafana as the only LAN-facing monitoring service on port `3000`
- Confirmed Prometheus is successfully scraping the MacBook's Node Exporter metrics
- Added Prometheus as a Grafana data source
- Verified live metrics are visible through Grafana Explore

## Current Monitoring Path

```text
MacBook metrics
      |
Node Exporter
127.0.0.1:9100
      |
Prometheus
127.0.0.1:9090
      |
Grafana
LAN :3000
      |
ThinkPad / Huawei MatePad
```

## Current State

The monitoring stack is operational and ready for dashboard development.

No Proxmox or OPNsense monitoring has been added yet.

## Next Actions

- Build a MatePad-friendly infrastructure dashboard
- Add CPU, RAM, load, uptime, disk, and network panels
- Add Proxmox host and VM monitoring
- Add DNS/service health checks
- Later add OPNsense and broader infrastructure monitoring

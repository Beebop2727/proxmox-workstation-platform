# Dockur Windows + WinApps Proof of Concept — 2026-08-21

Today we began testing a lightweight Windows compatibility layer inside the Ubuntu workstation VM on Proxmox.

## Completed

- Confirmed nested KVM support inside Ubuntu VM 101.
- Confirmed Docker can access `/dev/kvm`.
- Deployed Dockur Windows 11 LTSC with persistent storage.
- Created a restricted Ubuntu ↔ Windows shared folder.
- Enabled RDP/RemoteApp support for future WinApps integration.
- Confirmed FreeRDP connectivity to the Windows guest.
- Worked around an initial FreeRDP graphics freeze using software GDI rendering.

## Current Architecture

```text
Proxmox Host
└── Ubuntu VM 101
    ├── Docker
    └── Dockur Windows 11 LTSC
        └── WinApps / RemoteApp (next)
```

The Windows environment is intended for Microsoft Office and other Windows-only utilities, while gaming remains isolated in the existing Windows gaming VM.

## Next Steps

- Increase Ubuntu VM 101 memory to 24 GB.
- Increase Dockur Windows resources to ~6 GB RAM / 3 vCPU.
- Tune FreeRDP graphics performance.
- Install and configure WinApps.
- Test seamless launch of individual Windows applications such as Word and Excel.
- Keep Windows access limited to a dedicated shared bridge directory rather than the full Linux home folder.

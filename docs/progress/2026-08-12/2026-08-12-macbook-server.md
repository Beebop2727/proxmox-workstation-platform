## Progress Report — 12 August 2026

### MacBook Pro 15,1 → Ubuntu Server

- Decided to **abandon macOS** as the primary OS after continued issues with macOS firewall/network filtering interfering with AdGuard Home.
- Chose **Ubuntu Server** as the replacement platform rather than continuing with Proxmox.
- Ubuntu Server has now been **successfully installed** on the MacBook Pro 15,1.
- The MacBook is now intended primarily as a **server node**, with the possibility of running additional services/VMs later.

### AdGuard Home / macOS troubleshooting

- Confirmed AdGuard Home itself was functioning locally.
- Identified that macOS's **Content Filter (CFIL)** was actively dropping inbound connections to AdGuard Home.
- Logs explicitly showed `CFIL: RECEIVED CFM_OP_DROP` against AdGuardHome on TCP port 80.
- Proton VPN's network extensions were investigated as a possible additional source of network filtering.
- Proton VPN was removed/reinstalled during troubleshooting, but the underlying macOS networking problem persisted.
- Ultimately decided that continuing to fight macOS was no longer worthwhile.

### Ubuntu Server GUI

- Ubuntu Server is now running with a GUI.
- Adjusted the terminal/console text sizing using the Server-oriented configuration rather than treating it as a standard Ubuntu Desktop installation.

### MacBook Wi-Fi

The current priority is getting the **Broadcom BCM4364 Wi-Fi** working offline.

- MacBook identified as:
  - **MacBook Pro 15,1**
  - Intel Core i9 8-core CPU
  - 16 GB RAM
  - 500 GB internal SSD
  - Broadcom BCM4364 Wi-Fi hardware
- Prepared a USB with an offline Broadcom firmware package:
  - `apple-bcm-firmware-14.0-1-any.pkg.tar.zst`
  - ~7.2 MB
- Created an installation script:
  - `install-bcm4364.sh`
- The USB contains a **17.7 GB writable partition**, giving us plenty of room for additional offline packages/drivers.
- The MacBook is currently booted into Ubuntu Server with the USB plugged in.
- **Next step:** identify the USB's device/partition on the MacBook, mount it, then install the BCM4364 firmware and test Wi-Fi.

### Current status

**MacBook:** Ubuntu Server installed ✅  
**GUI:** Working ✅  
**USB:** Prepared with Broadcom firmware ❌  
**Wi-Fi:** Not yet configured ❌  
**Next task:** Offline BCM4364 driver/firmware installation.

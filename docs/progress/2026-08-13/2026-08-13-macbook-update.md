# MacBook Pro 15,1 — Ubuntu Server Installation

## 2026-08-13 Progress

### Migration from macOS

- Decided to move away from macOS after persistent networking/firewall issues affecting the AdGuard Home deployment.
- Abandoned the Proxmox approach and instead selected **Ubuntu Server** as the operating system.
- The MacBook is being repurposed as a **server node**, with the option to run additional services and VMs in the future.
- macOS was removed from the internal drive as part of the migration.

### MacBook Hardware

- **Model:** MacBook Pro 15,1
- **CPU:** 8-Core Intel Core i9 @ 2.3 GHz
- **RAM:** 16 GB
- **Storage:** 500 GB internal SSD
- **Wi-Fi:** Broadcom BCM4364

### Ubuntu Server

- Ubuntu Server was successfully installed onto the MacBook.
- A GUI was added to the Server installation for easier administration.
- Terminal/console text size was adjusted using the Ubuntu Server console configuration.

### Offline Wi-Fi Driver Preparation

The MacBook initially required offline Wi-Fi firmware, so a separate USB was prepared.

USB was reformatted to **ext4** and currently contains:

```text
apple-bcm-firmware-14.0-1-any.pkg.tar.zst
zstd_1.5.7+dfsg-3_amd64.deb
install-bcm4364.sh
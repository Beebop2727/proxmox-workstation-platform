# RAM Upgrade – 18 August 2026

Installed **2 × 32 GB SK Hynix DDR4** DIMMs in slots **A2/B2**, upgrading the Proxmox workstation from **32 GB to 64 GB** total system memory.

## Installation & Testing

* BIOS successfully detected **65,536 MB / 64 GB**
* Memory running in **dual-channel**
* Operating speed confirmed at **3200 MT/s**
* Initial POST required memory retraining after installation
* CMOS was cleared after a failed training attempt
* System subsequently booted successfully into Windows
* Display configuration was restored after CMOS reset
* Games tested successfully at approximately **210–220 FPS**
* No immediate stability issues observed

## Notes

The previous **4 × 8 GB** kit has been left out for now to maintain a simpler and more stable **2 × 32 GB** configuration.

Clearing CMOS reset some BIOS settings, so **SVM/IOMMU and other Proxmox-related virtualization settings will need to be checked and re-enabled** before resuming VM/passthrough testing.

## Future Improvements

Testing the possibility of adding **2 x 8 GB** sticks of RAM to the PC to increase total to **80 GB**, although likely at a reduced RAM speed. This will serve as a temporary gap to increase RAM with existing parts, but only if stability is safe for the workstation. From initial testing, **64 GB** might be all that is needed for the time being with future upgrade options to **128 GB**

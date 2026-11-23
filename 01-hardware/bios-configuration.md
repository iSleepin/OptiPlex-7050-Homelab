
# BIOS Configuration

## System Information

| Setting | Value |
|---|---|
| System | Dell OptiPlex 7050 SFF |
| BIOS Version | 1.27.0 |
| BIOS Release Date | September 17, 2023 |
| Processor | Intel Core i7-7700 |

## Virtualization Configuration

The following virtualization features were verified in BIOS and through Windows system information before installing Proxmox VE.

| Feature | Status |
|---|---|
| Intel Virtualization Technology (VT-x) | Enabled |
| Intel VT for Directed I/O (VT-d) | Enabled |
| Second Level Address Translation (SLAT) | Supported |

### Why These Settings Matter

- **Intel VT-x:** Enables hardware-assisted CPU virtualization for virtual machines.
- **Intel VT-d:** Supports hardware-assisted I/O virtualization and device passthrough.
- **SLAT:** Improves memory virtualization performance.

These capabilities allow the OptiPlex 7050 to operate as a virtualization host.

## Boot Configuration

- Proxmox installation was started using UEFI USB boot.
- Proxmox VE was installed directly onto the internal SATA SSD.

## Verification

Virtualization support was checked before installing Proxmox VE.

The system successfully booted into Proxmox VE after installation.

## Notes

BIOS settings should be reviewed after firmware updates or hardware changes to ensure virtualization remains enabled.

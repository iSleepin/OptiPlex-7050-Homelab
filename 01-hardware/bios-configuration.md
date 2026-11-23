# BIOS Configuration

## System Information

| Setting | Value |
|---|---|
| Manufacturer | Dell |
| System Model | OptiPlex 7050 SFF |
| BIOS Version | 1.27.0 |
| BIOS Release Date | September 17, 2023 |

## Virtualization Configuration

The following virtualization settings were checked in the Dell BIOS configuration utility.

| BIOS Setting | Configuration |
|---|---|
| Intel Virtualization Technology (VT-x) | Enabled |
| VT for Direct I/O (VT-d) | Enabled |

## Virtualization Support Verification

Windows PowerShell was used to verify processor virtualization capabilities.

| Feature | Result |
|---|---|
| Virtualization Firmware Enabled | True |
| Second Level Address Translation (SLAT) | Supported |
| Intel VT-x | Enabled |
| Intel VT-d | Enabled |

## Configuration Purpose

Virtualization support is required for the planned Proxmox VE hypervisor deployment.

- VT-x provides hardware-assisted CPU virtualization.
- VT-d provides hardware support for I/O virtualization and device passthrough.
- SLAT improves memory virtualization performance.

## Status

- [x] BIOS version documented
- [x] Intel VT-x verified enabled
- [x] Intel VT-d verified enabled
- [x] SLAT support verified

## Notes

BIOS settings were checked before beginning the Proxmox installation process.

No other BIOS settings were modified as part of this verification.

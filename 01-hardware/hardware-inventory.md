# Hardware Inventory

## System Overview

| Component | Specification |
|---|---|
| Manufacturer | Dell |
| Model | OptiPlex 7050 |
| Form Factor | Small Form Factor (SFF) |
| Operating System | Windows 10 Pro 22H2 (temporary installation) |
| CPU | Intel Core i7-7700 |
| CPU Architecture | 64-bit |
| CPU Cores | 4 |
| CPU Threads | 8 |
| CPU Base Frequency | 3.60 GHz |
| RAM | 32 GB DDR4 |
| RAM Configuration | 2 × 16 GB |
| RAM Speed | 2667 MHz |
| Primary Storage | KingFast 1 TB SATA SSD |
| Reported Storage Capacity | 953.87 GB |
| Graphics | Intel HD Graphics 630 |
| Network Adapter | Intel Ethernet Connection (5) I219-LM |
| Network Speed | 1 Gbps |
| BIOS Version | Dell 1.27.0 |
| BIOS Release Date | September 17, 2023 |

## Processor and Virtualization

- Processor: Intel Core i7-7700
- Physical cores: 4
- Logical processors: 8
- Intel Virtualization Technology (VT-x): Enabled
- Intel VT for Direct I/O (VT-d): Enabled
- Second Level Address Translation (SLAT): Supported

Virtualization settings were verified through Windows PowerShell and the Dell BIOS configuration utility.

## Memory Configuration

| Memory Slot | Capacity | Speed |
|---|---|---|
| DIMM2 | 16 GB | 2667 MHz |
| DIMM4 | 16 GB | 2667 MHz |
| **Total** | **32 GB** | **2667 MHz** |

## Storage

- Drive manufacturer/identifier: KingFast
- Drive type: SATA SSD
- Advertised capacity: 1 TB
- Reported capacity: 953.87 GB
- Partition style: GPT
- Windows-reported health: Healthy
- Operational status: OK

See [Storage Health](storage-health.md) for additional details.

## Networking

- Network adapter: Intel Ethernet Connection (5) I219-LM
- Connection type: Wired Ethernet
- Link speed: 1 Gbps
- Connection status during verification: Up

## BIOS Configuration

- BIOS manufacturer: Dell Inc.
- BIOS version: 1.27.0
- BIOS release date: September 17, 2023
- Intel Virtualization Technology (VT-x): Enabled
- Intel VT for Direct I/O (VT-d): Enabled

See [BIOS Configuration](bios-configuration.md) for details.

## Intended Homelab Workloads

The system is intended to host:

- Proxmox VE virtualization
- Linux and Windows Server virtual machines
- Docker and Docker Compose
- DNS and network services
- Pi-hole or AdGuard
- Tailscale remote access
- Immich photo management
- Jellyfin or Plex media streaming
- Minecraft game servers
- Nextcloud and Syncthing
- Proxmox Backup Server
- Uptime Kuma and Beszel monitoring

These are planned workloads and should not be considered deployed until individually installed and tested.

## Hardware Verification Status

- [x] System model identified
- [x] CPU specifications verified
- [x] RAM capacity and configuration verified
- [x] SSD identified
- [x] Network adapter and link speed verified
- [x] BIOS version recorded
- [x] Intel VT-x enabled
- [x] Intel VT-d enabled
- [x] SLAT support confirmed
- [ ] Detailed SSD SMART health assessment
- [ ] Additional hardware inspection

## Notes

Hardware specifications were collected using Windows PowerShell, WMI/CIM, and the Dell BIOS configuration utility.

The existing Windows installation is temporary and will be replaced with Proxmox VE during the next project phase.


# Hardware Inventory

## System Overview

| Component | Specification |
|---|---|
| Manufacturer | Dell |
| Model | OptiPlex 7050 Small Form Factor |
| CPU | Intel Core i7-7700 |
| CPU Architecture | 64-bit |
| Physical Cores | 4 |
| Logical Threads | 8 |
| Base Clock | 3.60 GHz |
| RAM | 32 GB DDR4 |
| Memory Configuration | 2 x 16 GB |
| Memory Speed | 2667 MHz |
| Primary Storage | 1 TB KingFast SATA SSD |
| Graphics | Intel HD Graphics 630 |
| Network Adapter | Intel Ethernet Connection I219-LM |
| Network Speed | 1 Gbps |
| Optical Drive | DVD±RW |

## Storage

### Primary SSD

- Manufacturer: KingFast
- Capacity: 1 TB
- Interface: SATA
- Partition scheme before Proxmox installation: GPT
- Operating system: Proxmox VE

The SSD was dedicated to the Proxmox installation. The previous Windows installation and its partitions were erased.

## Memory

- Total installed memory: 32 GB
- Configuration: 2 x 16 GB DDR4
- Reported speed: 2667 MHz
- Installed in DIMM2 and DIMM4

## Networking

- Adapter: Intel Ethernet Connection (5) I219-LM
- Link speed: 1 Gbps
- Connection type: Ethernet
- Proxmox management IP: `<PROXMOX-LAN-IP>`
- Default gateway: `<LAN-GATEWAY-IP>`

## Intended Use

The system is being used as a home virtualization server for:

- Proxmox VE
- Linux virtual machines
- Windows Server virtual machines
- Docker and Docker Compose
- Network infrastructure services
- Self-hosted applications
- Monitoring and backup services
- Game server hosting

These are project goals, not a claim that every service is currently deployed.

## Hardware Considerations

- The Intel Core i7-7700 provides 4 cores and 8 threads.
- The 32 GB RAM configuration provides a reasonable starting point for multiple virtual machines.
- CPU and memory allocations should be monitored to avoid excessive resource overcommitment.
- Additional storage may be needed for backups and larger application datasets.

## Notes

Hardware specifications were collected through Windows system information, PowerShell, and BIOS verification before installing Proxmox VE.

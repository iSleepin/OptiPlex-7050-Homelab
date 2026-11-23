# Dell OptiPlex 7050 Homelab

A self-hosted homelab project built using a Dell OptiPlex 7050 Small Form Factor (SFF). The goal is to gain practical experience with IT infrastructure, virtualization, networking, Linux and Windows Server administration, containerization, storage, security, and self-hosted applications.

This project also serves as a technical portfolio documenting hands-on learning, configurations, troubleshooting, and implementation progress.

## Hardware Specifications

| Component | Specification |
|---|---|
| Manufacturer | Dell |
| Model | OptiPlex 7050 |
| Form Factor | Small Form Factor (SFF) |
| CPU | Intel Core i7-7700 |
| CPU Cores / Threads | 4 Cores / 8 Threads |
| CPU Base Frequency | 3.60 GHz |
| RAM | 32 GB DDR4 |
| RAM Configuration | 2 × 16 GB |
| RAM Speed | 2667 MHz |
| Storage | KingFast 1 TB SATA SSD |
| Reported SSD Capacity | 953.87 GB |
| Graphics | Intel HD Graphics 630 |
| Network Adapter | Intel Ethernet Connection (5) I219-LM |
| Network Speed | 1 Gbps |
| BIOS | Dell 1.27.0 |

### Virtualization Support

| Feature | Status |
|---|---|
| Intel VT-x | Enabled |
| Intel VT-d | Enabled |
| SLAT | Supported |

## Project Goals

- Learn and implement bare-metal virtualization using Proxmox VE.
- Deploy and manage Linux and Windows Server virtual machines.
- Develop practical networking and infrastructure administration skills.
- Configure DNS, DHCP, and remote access services.
- Learn Docker and Docker Compose.
- Build self-hosted applications and services.
- Implement storage, backup, and recovery solutions.
- Configure system monitoring and dashboards.
- Practice security hardening and troubleshooting.
- Document technical work for professional portfolio development.

## Planned Technologies

### Virtualization
- Proxmox VE
- Linux Virtual Machines
- Windows Server Virtual Machines

### Networking
- DNS
- DHCP
- Pi-hole or AdGuard
- Tailscale
- Internal DNS

### System Administration
- Linux Administration
- Windows Server
- Active Directory
- Group Policy
- User and Permission Management

### Containers and Applications
- Docker
- Docker Compose
- Immich
- Jellyfin or Plex
- Nextcloud
- Syncthing
- AMP
- Minecraft Server
- Playit.gg

### Storage and Backups
- Proxmox Backup Server
- Network File Sharing
- SMB
- Storage Management
- Backup and Restore Testing

### Monitoring and Dashboards
- Uptime Kuma
- Beszel
- Homepage, Homarr, or Heimdall

## Project Roadmap

| Phase | Description | Status |
|---|---|---|
| 0 | Hardware Preparation and Verification | Complete |
| 1 | Proxmox VE Installation | In Progress |
| 2 | Networking and Network Services | Planned |
| 3 | Linux Administration and Docker | Planned |
| 4 | Windows Server and Active Directory | Planned |
| 5 | Storage, NAS, and Backups | Planned |
| 6 | Media and Game Server Applications | Planned |
| 7 | Monitoring and Dashboards | Planned |
| 8 | Security Hardening | Planned |
| 9 | Troubleshooting and Incident Documentation | Planned |
| 10 | Portfolio Documentation and Improvements | Planned |

## Project Status

**Current Phase: Phase 1 — Proxmox VE Installation Preparation**

### Completed

- [x] GitHub repository created
- [x] Initial project documentation established
- [x] Hardware inventory documented
- [x] Intel Core i7-7700 CPU verified
- [x] 32 GB DDR4 RAM configuration verified
- [x] KingFast 1 TB SATA SSD identified
- [x] Gigabit Ethernet adapter verified
- [x] BIOS version documented
- [x] Intel VT-x enabled
- [x] Intel VT-d enabled
- [x] SLAT support confirmed
- [x] Proxmox VE installation USB created using Rufus

### Upcoming

- [ ] Detailed SSD SMART assessment
- [ ] Proxmox network configuration planning
- [ ] Proxmox VE installation
- [ ] Configure Proxmox networking
- [ ] Deploy virtual machines and containers
- [ ] Configure network services
- [ ] Deploy Docker applications
- [ ] Configure storage and backups
- [ ] Deploy monitoring tools
- [ ] Implement security hardening

## Repository Structure

```text
optiplex-7050-homelab/
│
├── README.md
├── CHANGELOG.md
│
├── 01-hardware/
│   ├── hardware-inventory.md
│   ├── upgrade-log.md
│   ├── bios-configuration.md
│   └── storage-health.md
│
├── 02-proxmox/
│   ├── installation.md
│   ├── configuration.md
│   └── vm-inventory.md
│
├── 03-networking/
│   ├── network-overview.md
│   ├── ip-address-plan.md
│   ├── dns-dhcp.md
│   └── tailscale.md
│
├── 04-linux/
├── 05-windows-server/
├── 06-docker/
├── 07-storage/
├── 08-backups/
├── 09-applications/
├── 10-monitoring/
├── 11-security/
├── 12-troubleshooting/
├── 13-network-diagrams/
└── 14-resume-evidence/
```

## Documentation

- [Hardware Inventory](01-hardware/hardware-inventory.md)
- [BIOS Configuration](01-hardware/bios-configuration.md)
- [Storage Health](01-hardware/storage-health.md)
- [Changelog](CHANGELOG.md)

## Learning Outcomes

This project is intended to develop practical experience in:

- Hardware identification and troubleshooting
- Hypervisor installation and management
- Virtual machine provisioning
- Network configuration and troubleshooting
- Linux and Windows Server administration
- Active Directory and Group Policy
- Container management
- Storage and backup administration
- Security configuration
- Monitoring and incident troubleshooting

## Disclaimer

This repository documents a personal homelab and independent learning project. Technologies listed as planned are not necessarily installed or operational. Implementation status will be updated as each service is configured and tested.

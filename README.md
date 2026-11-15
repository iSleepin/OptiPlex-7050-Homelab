# Dell OptiPlex 7050 Homelab

## Overview

This project documents the planning, deployment, configuration, and maintenance of a personal homelab built using a Dell OptiPlex 7050 Small Form Factor (SFF) desktop.

The goal is to develop practical, hands-on experience with IT infrastructure, systems administration, networking, virtualization, storage, cybersecurity, and self-hosted applications.

This homelab is designed as an educational environment for building and documenting technical skills relevant to IT Support, Help Desk, and Systems Administration roles.

## Hardware

| Component | Specification |
|---|---|
| System | Dell OptiPlex 7050 SFF |
| Processor | Intel Core i7-7700 |
| CPU Cores / Threads | 4 Cores / 8 Threads |
| Memory | 32 GB DDR4 RAM |
| Primary Storage | 1 TB SSD |
| Graphics | Intel HD Graphics 630 |
| Networking | Gigabit Ethernet |
| Operating System | Windows 10 Pro 22H2 |

## Planned Technologies

### Virtualization
- Proxmox VE
- Linux Virtual Machines
- Windows Server Virtual Machine

### Networking
- DNS and DHCP
- Pi-hole / AdGuard
- Tailscale
- InternalDNS

### Systems Administration
- Linux Administration
- Windows Server
- Active Directory
- Group Policy
- User and Permission Management

### Containers and Applications
- Docker
- Docker Compose
- Immich
- Nextcloud
- Syncthing
- Jellyfin / Plex
- AMP Game Server

### Storage and Backups
- Proxmox Backup Server
- Network File Sharing
- Automated Backups
- Backup Restoration Testing

### Monitoring and Management
- Uptime Kuma
- Beszel
- Homepage / Homarr / Heimdall

## Project Objectives

- Deploy and manage a bare-metal virtualization environment.
- Configure and administer Linux and Windows Server systems.
- Build and troubleshoot virtual networks and infrastructure services.
- Implement DNS, DHCP, VPN, and network security.
- Deploy and maintain Docker containers.
- Configure centralized storage, file sharing, and backups.
- Host self-managed applications and game servers.
- Monitor system performance, availability, and resource utilization.
- Document configurations, troubleshooting procedures, and lessons learned.

## Repository Structure

| Directory | Description |
|---|---|
| `01-hardware/` | Hardware inventory, BIOS, and upgrades |
| `02-proxmox/` | Proxmox installation and configuration |
| `03-networking/` | Network topology, DNS, DHCP, and VPN |
| `04-linux/` | Linux administration |
| `05-windows-server/` | Active Directory and Windows Server |
| `06-docker/` | Docker and container management |
| `07-storage/` | Storage and file-sharing services |
| `08-backups/` | Backup strategy and recovery |
| `09-applications/` | Self-hosted applications |
| `10-monitoring/` | Monitoring and dashboards |
| `11-security/` | Security configuration and hardening |
| `12-troubleshooting/` | Troubleshooting and incident documentation |
| `13-network-diagrams/` | Network diagrams and infrastructure layouts |
| `14-resume-evidence/` | Technical skills and project accomplishments |

## Project Status

**Current Phase:** Hardware Preparation and Inventory

| Phase | Status |
|---|---|
| Project planning and repository setup | Complete |
| Hardware inventory | Complete |
| Hardware and BIOS verification | Pending |
| Proxmox VE installation | Pending |
| Virtual machine deployment | Pending |
| Networking and infrastructure services | Pending |
| Linux and Windows Server administration | Pending |
| Docker and application deployment | Pending |
| Storage and backup configuration | Pending |
| Monitoring and security | Pending |
| Documentation and portfolio development | In Progress |

For detailed progress, see [CHANGELOG.md](CHANGELOG.md).

## Documentation

- [Hardware Inventory](01-hardware/hardware-inventory.md)
- [Hardware Upgrade Log](01-hardware/upgrade-log.md)
- [BIOS Configuration](01-hardware/bios-configuration.md)
- [Proxmox Documentation](02-proxmox/)
- [Networking Documentation](03-networking/)
- [Linux Administration](04-linux/)
- [Windows Server](05-windows-server/)
- [Docker Documentation](06-docker/)
- [Storage Documentation](07-storage/)
- [Backup Documentation](08-backups/)
- [Application Documentation](09-applications/)
- [Monitoring Documentation](10-monitoring/)
- [Security Documentation](11-security/)
- [Troubleshooting Logs](12-troubleshooting/)

## Learning Outcomes

This project is intended to demonstrate practical experience with:

- Virtualization and infrastructure management
- Operating system installation and administration
- TCP/IP networking, DNS, and DHCP
- Active Directory and Group Policy
- Linux command-line administration
- Docker containerization
- Storage management and backup recovery
- Network security and remote access
- System monitoring and troubleshooting
- Technical documentation

## Disclaimer

This is an independent educational homelab project. All configurations, deployments, troubleshooting activities, and results will be documented as they are completed.

Technologies listed as planned are not necessarily deployed or operational.

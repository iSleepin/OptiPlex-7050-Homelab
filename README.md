
# OptiPlex 7050 Homelab

A self-hosted homelab built using a Dell OptiPlex 7050 Small Form Factor. This project documents the deployment and administration of a Proxmox VE environment, Linux and Windows virtual machines, networking, containerization, storage, security, and monitoring.

The goal is to develop practical IT infrastructure, troubleshooting, networking, and systems administration skills through hands-on projects.

## Hardware

| Component | Specification |
|---|---|
| System | Dell OptiPlex 7050 SFF |
| CPU | Intel Core i7-7700 |
| CPU Configuration | 4 cores / 8 threads |
| Memory | 32 GB DDR4 (2 x 16 GB) |
| Memory Speed | 2667 MHz |
| Primary Storage | 1 TB KingFast SATA SSD |
| Network | Intel I219-LM Gigabit Ethernet |
| Graphics | Intel HD Graphics 630 |

See [Hardware Inventory](01-hardware/hardware-inventory.md) for details.

## Hypervisor

- Proxmox Virtual Environment
- Bare-metal installation
- ext4 filesystem with LVM-thin storage
- Management address: `192.168.68.200`
- Hostname: `pve`

See the [Proxmox installation documentation](02-proxmox/installation.md).

## Project Roadmap

| Phase | Area | Status |
|---|---|---|
| 0 | Hardware preparation and inventory | Complete |
| 1 | Proxmox VE installation | Complete |
| 2 | Networking infrastructure | In Progress |
| 3 | Linux administration and Docker | Planned |
| 4 | Windows Server and Active Directory | Planned |
| 5 | Storage and backups | Planned |
| 6 | Media and game servers | Planned |
| 7 | Monitoring and dashboards | Planned |
| 8 | Security hardening | Planned |
| 9 | Troubleshooting documentation | Planned |
| 10 | Portfolio development | Planned |

## Planned Services

The following services are planned for future deployment. They are not yet configured unless explicitly documented elsewhere.

- Pi-hole or AdGuard Home
- Tailscale
- Docker and Docker Compose
- Linux virtual machines
- Windows Server and Active Directory
- Immich
- Jellyfin
- Minecraft server
- Nextcloud
- Syncthing
- Proxmox Backup Server
- Uptime Kuma
- Beszel
- Homepage or Homarr

## Repository Structure

```text
OptiPlex-7050-Homelab/
├── README.md
├── CHANGELOG.md
├── 01-hardware/
├── 02-proxmox/
├── 03-networking/
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

## Learning Objectives

- Deploy and manage a bare-metal hypervisor.
- Configure virtual machines and virtual networking.
- Administer Linux and Windows environments.
- Practice DNS, DHCP, and network troubleshooting.
- Deploy and manage containerized applications.
- Implement storage, backup, and recovery procedures.
- Monitor infrastructure and troubleshoot service failures.
- Document technical work and operational procedures.

## Documentation

Each section of this repository records configurations, procedures, testing results, and lessons learned as the homelab develops.

Sensitive information such as passwords, API keys, tokens, serial numbers, and private credentials will not be published.

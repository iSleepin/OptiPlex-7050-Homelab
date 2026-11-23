# Changelog

All notable changes to the OptiPlex 7050 Homelab project are documented here.

## [2026-11-23] - AMP and Minecraft Server Milestone

### Added

- Added `02-proxmox/vm-inventory.md` documenting the deployed virtual machines.
- Created the `09-applications` documentation directory.
- Added `09-applications/amp.md` documenting AMP installation and configuration.
- Added `09-applications/minecraft-server.md` documenting the Linggango Minecraft server.
- Created the `08-backups` documentation directory.
- Added `08-backups/backup-strategy.md` documenting the current local backup configuration.

### Infrastructure

- Created Debian 13 VM 101 (`amp-server`) with 4 vCPUs, 12 GB RAM, and a 100 GB virtual disk.
- Installed CubeCoders AMP Instance Manager.
- Configured AMP in Standalone mode.
- Installed Podman support, Java runtimes, and SteamCMD 32-bit libraries.
- Created and started a Minecraft Java server instance through AMP.
- Installed the Linggango modpack server files.
- Configured Minecraft 1.20.1 with Forge 47.4.20 and Eclipse Temurin 17.
- Configured AMP local backup retention settings.

### Backup Configuration

- Configured a 24-backup count limit.
- Configured a 20,280 MB total backup storage limit.
- Configured a 10,140 MB individual backup limit.
- Selected Delete Single Oldest as the replacement policy.
- Selected Optimal compression.
- Set a five-second quiesce delay.

The hourly backup schedule and successful backup creation have not yet been verified. Independent backup storage and restore testing remain future work.

### Documentation

- Updated the project README with the current VM inventory and application deployment.
- Documented AMP and Minecraft configuration details.
- Distinguished configured backup settings from verified backup operations.

## [2026-11-23] - Networking and DNS Milestone

### Added

- Created the `03-networking` documentation directory.
- Added `dns-dhcp.md` documenting Technitium DNS and DHCP configuration.
- Added `network-overview.md` documenting network architecture and topology.
- Added `06-docker/containers.md` documenting the Docker environment and deployed containers.
- Documented the internal `home.arpa` DNS zone and service records.

### Networking

- Deployed Technitium DNS Server using Docker Compose on Debian 13.
- Configured Cloudflare DNS-over-HTTPS as the upstream resolver.
- Configured the TP-Link Deco M9 Plus to distribute Technitium as the primary DNS server through DHCP.
- Created internal DNS records for Proxmox VE and Technitium.
- Verified external DNS resolution over UDP and TCP.
- Verified internal DNS resolution from a Windows client.
- Verified browser access using internal DNS hostnames.

### Documentation

- Updated the project README with current infrastructure and project progress.
- Documented verified network settings and service access URLs.
- Distinguished completed implementations from planned services.

## [2026-11-22] - Proxmox VE Installation Milestone

### Added

- Documented hardware inventory and system specifications.
- Documented BIOS configuration and virtualization settings.
- Documented Proxmox VE installation.
- Documented initial Proxmox configuration and verification.

### Infrastructure

- Installed Proxmox VE bare metal on the Dell OptiPlex 7050 SFF.
- Configured the Proxmox management IP address.
- Verified gateway connectivity and external DNS resolution.
- Verified CPU, memory, and storage availability.

### Documentation

- Created the initial project documentation structure.
- Added hardware and Proxmox installation documentation.

## Initial Project

### Infrastructure

- Selected the Dell OptiPlex 7050 SFF as the primary homelab server.
- Established the project roadmap and infrastructure goals.

### Planned Work

- Linux administration and Docker services.
- Windows Server and Active Directory.
- Storage and backup infrastructure.
- Media and game server hosting.
- Monitoring and security improvements.
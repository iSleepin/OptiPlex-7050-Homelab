# Changelog

All notable changes to the OptiPlex 7050 Homelab project are documented here.

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

# Changelog

All notable changes to the OptiPlex 7050 Homelab project are documented here.

## [2026-11-25] - Monitoring and Observability Milestone

### Added

- Documented the existing Uptime Kuma deployment on the Debian services VM, managed with Docker Compose.
- Documented five configured Uptime Kuma monitors for Proxmox VE, Homepage, the Technitium web interface, AMP, and the Minecraft Java server.
- Created the `homelab` Uptime Kuma status page for service availability reporting.
- Deployed Beszel Hub on the Debian services VM using Docker Compose.
- Deployed the Beszel agent on the Debian services VM as a Docker container.
- Deployed the Beszel agent on the AMP VM as a standalone Linux binary managed by systemd.
- Integrated Beszel overview and per-system widgets into Homepage for both Debian VMs.
- Documented the existing Uptime Kuma status widget in Homepage.
- Added `07-monitoring/monitoring.md` documenting the monitoring architecture, deployment, and verification.

### Verification

- Confirmed all five Uptime Kuma monitors reported Up during verification.
- Confirmed Beszel reported both Debian VMs online and collected CPU, memory, disk, average load, network, and uptime metrics.
- Confirmed Homepage displayed the Beszel overview and live metrics for `debian-services` and `amp-server`.
- Rebooted the AMP VM and verified that its Beszel systemd service started automatically.
- Verified the AMP VM agent re-established its WebSocket connection to Beszel after network connectivity became available during boot.
- Stored Homepage's Beszel API credentials in the local protected environment file; credential values were not added to repository documentation.

### Documentation

- Updated the README to reflect the completed monitoring phase and Homepage integrations.
- Updated the Docker inventory with Uptime Kuma and Beszel Hub.
- Updated the network overview to show the monitoring services and agents on their respective VMs.
- Kept private LAN addresses, API credentials, tokens, and other secrets out of the public documentation.


## [2026-11-23] - Public Documentation Security and Accuracy Review

### Security and Documentation

- Replaced private LAN and Tailscale addresses with descriptive placeholders in public documentation.
- Removed the actual Windows administration hostname and private MagicDNS hostname from the public Tailscale documentation.
- Located Tailscale documentation under `03-networking/tailscale.md` to match the networking documentation structure and README links.
- Updated DNS documentation to reflect the enabled HaGeZi Multi Normal block list and its 24-hour refresh interval.
- Updated the network overview to include both Debian VMs, Homepage, Technitium, AMP, Tailscale Serve, and the Playit Minecraft tunnel.
- Corrected stale future-work statements for already deployed remote access and DNS filtering.
- Clarified that AMP's hourly backup schedule and successful backup creation remain unverified.
- Corrected the README repository tree to list only files currently present in the repository.

## [2026-11-23] - Docker Startup Reliability and Documentation

### Troubleshooting and Infrastructure

- Investigated Homepage and Technitium containers failing to start after a Proxmox host reboot.
- Identified a startup race in which Docker attempted to bind published ports before the Debian VM received its reserved LAN address.
- Enabled `ifupdown-wait-online.service` as an initial troubleshooting attempt; this did not guarantee the DHCP address was available.
- Added a Docker systemd drop-in at `/etc/systemd/system/docker.service.d/wait-for-lan-ip.conf`.
- Configured Docker to wait up to 120 seconds for the expected IPv4 address on `ens18` before starting.
- Reloaded systemd and verified the Docker startup override.
- Reboot-tested the Debian services VM and confirmed both containers started automatically.
- Verified Homepage reported a healthy status and Technitium started successfully.
- Verified both web interfaces were reachable from a Windows client after reboot.
- Confirmed no Docker host-port binding errors appeared in the filtered boot journal.

### Documentation

- Expanded `06-docker/containers.md` with the Homepage deployment and Docker startup troubleshooting procedure.
- Documented the initial unsuccessful wait-online approach, final systemd fix, verification steps, and lessons learned.
- Used documentation-only IP placeholders to avoid publishing the actual LAN address.

## [2026-11-23] - Remote Access Milestone

### Added

- Added `03-networking/tailscale.md` documenting private remote administration.
- Added `09-applications/playit.md` documenting public Minecraft connectivity.
- Documented Tailscale Serve for private HTTPS access to Technitium DNS.
- Documented the Playit.gg Minecraft Java tunnel.
- Updated `09-applications/minecraft-server.md` with public remote access configuration.
- Updated `09-applications/amp.md` with private administration and remote-access architecture.
- Updated the project README with the remote-access infrastructure.

### Remote Access Infrastructure

- Installed and connected Tailscale on the Proxmox host.
- Installed and connected Tailscale on Debian services VM 100.
- Installed and connected Tailscale on AMP VM 101.
- Connected the Windows administration computer to the same tailnet.
- Verified remote TCP connectivity to the Proxmox web interface on port 8006.
- Verified remote TCP connectivity to AMP on port 8080.
- Configured Tailscale Serve to provide private HTTPS access to Technitium DNS.
- Verified Tailscale Serve connectivity over TCP port 443.

### Public Minecraft Connectivity

- Installed the Playit.gg agent on the AMP VM.
- Registered and connected the agent as `amp-server-minecraft`.
- Created a Minecraft Java tunnel named `Linggango-Minecraft-Java`.
- Configured the tunnel to forward connections to `127.0.0.1:25565`.
- Verified the Minecraft tunnel was operational.
- Successfully tested public Minecraft connectivity from outside the home network.
- Confirmed that router port forwarding was not required.

### Security

- Separated private administrative access from public Minecraft connectivity.
- Kept Proxmox, AMP, and Technitium administration outside the public Playit tunnel.
- Kept Tailscale Funnel disabled.
- Excluded actual public hostnames, private Tailscale addresses, and authentication secrets from repository documentation.
- Documented the use of Tailscale for private administration and Playit.gg exclusively for public Minecraft access.

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



# OptiPlex 7050 Homelab

A personal homelab project built around a Dell OptiPlex 7050 SFF running Proxmox VE. The goal is to develop practical IT infrastructure, Linux administration, networking, virtualization, Docker, and systems administration skills through hands-on projects.

This repository documents the hardware, installation process, configurations, troubleshooting, and lessons learned as the homelab evolves.

## Project Goals

- Gain hands-on experience with virtualization and server administration.
- Develop Linux and Windows Server administration skills.
- Learn networking, DNS, DHCP, and network troubleshooting.
- Deploy and manage applications using Docker and Docker Compose.
- Build experience with storage, backups, and disaster recovery.
- Implement monitoring, security, and remote administration.
- Maintain technical documentation that demonstrates practical IT skills.

## Hardware

### Primary Server

| Component | Specification |
|---|---|
| System | Dell OptiPlex 7050 SFF |
| CPU | Intel Core i7-7700 |
| CPU Cores / Threads | 4 cores / 8 threads |
| Base Clock | 3.60 GHz |
| RAM | 32 GB DDR4 |
| Memory Configuration | 2 × 16 GB |
| Memory Speed | 2667 MHz |
| Storage | 1 TB KingFast SATA SSD |
| Integrated Graphics | Intel HD Graphics 630 |
| Ethernet | Intel I219-LM Gigabit Ethernet |
| BIOS | Dell Version 1.27.0 |
| Virtualization | Intel VT-x and VT-d enabled |

The OptiPlex 7050 SFF provides a compact platform for learning virtualization and hosting multiple services. VM allocations are planned around the host's 32 GB RAM and 8 logical CPU threads.

## Software and Infrastructure

| Component | Technology |
|---|---|
| Hypervisor | Proxmox VE |
| Virtual Machine OS | Debian 13 |
| Container Platform | Docker Engine |
| Container Management | Docker Compose |
| DNS Server | Technitium DNS Server |
| Dashboard | Homepage |
| Game Server Management | CubeCoders AMP |
| Game Server | Minecraft Java Edition |
| Minecraft Mod Loader | Minecraft Forge |
| Private Remote Access | Tailscale |
| Public Game Server Access | Playit.gg |
| Router / DHCP | TP-Link Deco M9 Plus |
| Internal DNS Domain | `home.arpa` |

## Current Infrastructure

### Proxmox VE

Proxmox VE is installed directly on the OptiPlex 7050 and serves as the bare-metal hypervisor.

- Installation type: Bare metal
- Hostname: `pve`
- Management address: `<PROXMOX-LAN-IP>`
- Web interface: `https://proxmox.home.arpa:8006`
- Storage: Local storage and LVM-thin

### Virtual Machines

| VM ID | Hostname | Operating System | vCPU | RAM | Disk | Role |
|---|---|---|---:|---:|---:|---|
| 100 | `debian-services` | Debian 13 | 2 | 4 GB | 32 GB | Docker and network services |
| 101 | `amp-server` | Debian 13 | 4 | 12 GB | 100 GB | AMP and game servers |

Detailed VM configurations are documented in [`02-proxmox/vm-inventory.md`](02-proxmox/vm-inventory.md).

### Debian Services VM

VM 100 hosts Docker and network services.

| Property | Configuration |
|---|---|
| VM ID | 100 |
| Hostname | `debian-services` |
| Operating System | Debian 13 |
| vCPU | 2 |
| Memory | 4 GB |
| Virtual Disk | 32 GB |
| IP Address | `<DEBIAN-SERVICES-LAN-IP>` |
| Administration | SSH and Tailscale |

### Docker Environment

Docker Engine and Docker Compose are installed on the Debian services VM.

| Component | Version |
|---|---|
| Docker Engine | 29.10.0 |
| Docker Compose | 5.8.0 |

Docker services are deployed using Docker Compose with persistent configuration and data directories.

#### Deployed Containers

| Service | Container | Status |
|---|---|---|
| Technitium DNS Server | `technitium` | Running and verified after reboot |
| Homepage | `homepage` | Running and verified after reboot |

**Homepage Dashboard**

Homepage provides a centralized dashboard with shortcuts to the homelab's management interfaces and applications.

- Access: `http://<DEBIAN-SERVICES-LAN-IP>:3000`
- Configuration directory: `/opt/homepage/config/`
- Deployment directory: `/opt/homepage/`
- Container image: `ghcr.io/gethomepage/homepage:latest`

**Docker Startup Reliability**

Docker is configured to wait for the Debian services VM's reserved LAN address on `ens18` before starting. This prevents containers with LAN-bound published ports from failing during boot if Docker starts before DHCP has assigned the address.

The startup override is located at:

`/etc/systemd/system/docker.service.d/wait-for-lan-ip.conf`

The configuration waits up to 120 seconds for the expected IPv4 address before allowing Docker to start. It was verified through a VM reboot, after which both Homepage and Technitium started successfully.

See [`06-docker/containers.md`](06-docker/containers.md) for deployment, configuration, troubleshooting, and management details.

### AMP Game Server VM

VM 101 is dedicated to CubeCoders AMP and game-server management.

| Property | Configuration |
|---|---|
| VM ID | 101 |
| Hostname | `amp-server` |
| Operating System | Debian 13 |
| vCPU | 4 |
| Memory | 12 GB |
| Virtual Disk | 100 GB |
| IP Address | `<AMP-SERVER-LAN-IP>` |
| Management Software | CubeCoders AMP |
| AMP Web Interface | `http://<AMP-SERVER-LAN-IP>:8080` |

AMP is configured in Standalone mode and manages game-server instances on the Debian VM.

The VM was configured with Podman support, Java runtimes, and SteamCMD 32-bit libraries for future game-server flexibility.

See [`09-applications/amp.md`](09-applications/amp.md).

### Minecraft Server

A modded Minecraft Java server is managed through AMP.

| Property | Configuration |
|---|---|
| Modpack | Linggango |
| Minecraft Version | 1.20.1 |
| Forge Version | 47.4.20 |
| Java Runtime | Eclipse Temurin 17 |
| AMP Instance Management | AMP |
| Server VM | VM 101 |
| Local Server Address | `<AMP-SERVER-LAN-IP>:25565` |
| Public Connectivity | Playit.gg |

The server pack was transferred and extracted into the AMP instance directory. The server was successfully started through AMP.

Public connectivity was verified from outside the home network using Playit.gg without configuring router port forwarding.

See [`09-applications/minecraft-server.md`](09-applications/minecraft-server.md).

## Networking and DNS

**Status: Core DNS configuration and remote access completed**

The TP-Link Deco M9 Plus remains responsible for routing and DHCP. Technitium handles DNS resolution for network clients.

### Network Configuration

| Setting | Value |
|---|---|
| Network | `<LAN-SUBNET>` |
| Subnet Mask | `<SUBNET-MASK>` |
| Default Gateway | `<LAN-GATEWAY-IP>` |
| DHCP Server | TP-Link Deco M9 Plus |
| DHCP Pool | `<DHCP-POOL-START-IP>` – `<DHCP-POOL-END-IP>` |
| Primary DNS | `<DEBIAN-SERVICES-LAN-IP>` |

### DNS Infrastructure

Technitium DNS Server runs in Docker on the Debian services VM.

- DNS: TCP and UDP port 53
- Web administration: TCP port 5380
- Internal DNS zone: `home.arpa`
- Upstream resolver: Cloudflare DNS-over-HTTPS
- Primary upstream: `1.1.1.1`
- Secondary upstream: `1.0.0.1`

### Internal DNS Records

| Hostname | IP Address | Service |
|---|---|---|
| `proxmox.home.arpa` | `<PROXMOX-LAN-IP>` | Proxmox VE |
| `dns.home.arpa` | `<DEBIAN-SERVICES-LAN-IP>` | Technitium DNS |

### Service Access

| Service | Address |
|---|---|
| Proxmox VE | `https://proxmox.home.arpa:8006` |
| Technitium DNS | `http://dns.home.arpa:5380` |
| Homepage | `http://<DEBIAN-SERVICES-LAN-IP>:3000` |
| AMP | `http://<AMP-SERVER-LAN-IP>:8080` |

### DNS Verification

The following tests have been completed successfully:

- External DNS resolution through Technitium.
- DNS resolution over UDP and TCP.
- Windows client DNS resolution through Technitium.
- DHCP distribution of the Technitium DNS address.
- Internal DNS resolution for Proxmox VE.
- Internal DNS resolution for Technitium.
- Browser access using internal DNS hostnames.

## Remote Access

The homelab uses separate services for private administration and public application access.

### Tailscale — Private Remote Administration

Tailscale provides private remote connectivity between the following devices:

- Proxmox host (`pve`)
- Debian services VM (`debian-services`)
- AMP VM (`amp-server`)
- Windows administration computer

Tailscale is used to access administrative services without exposing management interfaces directly to the public internet.

Verified remote connectivity includes:

| Service | Port | Access |
|---|---:|---|
| Proxmox VE | 8006 | Tailscale |
| AMP management | 8080 | Tailscale |
| Technitium DNS | 443 | Tailscale Serve |

### Tailscale Serve

Tailscale Serve provides private HTTPS access to the Technitium DNS web interface.

The service proxies requests to the local Technitium web interface on the Debian services VM.

The HTTPS endpoint is accessible only within the tailnet. Tailscale Funnel is not enabled.

Actual Tailscale IP addresses and the private MagicDNS hostname are intentionally excluded from this public repository.

### Playit.gg — Public Minecraft Connectivity

Playit.gg provides public connectivity to the Linggango Minecraft server.

The Playit agent runs on the AMP VM and forwards incoming Minecraft connections to the local server at:

```text
127.0.0.1:25565
```

| Setting | Configuration |
|---|---|
| Agent | `amp-server-minecraft` |
| Tunnel | `Linggango-Minecraft-Java` |
| Tunnel Type | Minecraft Java |
| Local Destination | `127.0.0.1:25565` |
| Public Hostname | `your-server.example.com` |

The public hostname is a fictional placeholder and is not the actual assigned address.

Public connectivity was tested successfully from outside the home network.

### Network Security

- Tailscale is used for private administrative access.
- Playit.gg is used exclusively for public Minecraft connectivity.
- AMP and Proxmox management interfaces are not exposed through Playit.
- Technitium administration remains private through LAN and Tailscale access.
- No router port forwarding is configured for these services.
- Public hostnames, authentication tokens, and agent secrets are excluded from repository documentation.

See [`03-networking/tailscale.md`](03-networking/tailscale.md) and [`09-applications/playit.md`](09-applications/playit.md).

## Backup Infrastructure

AMP's local Minecraft backup retention settings have been configured.

- Backup count limit: 24
- Backup space limit: 20,280 MB
- Individual backup limit: 10,140 MB
- Replacement policy: Delete Single Oldest
- Compression: Optimal
- Local storage path: `./Backups/`

The hourly backup schedule and successful backup creation have not yet been verified. Backups currently use storage within the AMP VM and are not an independent copy of the VM.

See [`08-backups/backup-strategy.md`](08-backups/backup-strategy.md).

## Project Progress

| Phase | Description | Status |
|---|---|---|
| 0 | Hardware preparation and inventory | Complete |
| 1 | Proxmox VE installation and verification | Complete |
| 2 | Networking and core DNS configuration | Complete |
| 3 | Linux administration and initial Docker setup | In Progress |
| 4 | Windows Server and Active Directory | Planned |
| 5 | Storage, NAS, and backup infrastructure | In Progress |
| 6 | Media and game server applications | In Progress |
| 7 | Monitoring and dashboards | In Progress |
| 8 | Security hardening | Planned |
| 9 | Troubleshooting and incident documentation | Ongoing |
| 10 | Portfolio documentation and project polish | Ongoing |

## Planned Services

The following services are potential future additions. They are not considered deployed until installation and testing are completed.

### Networking and Remote Access

- Additional internal DNS records
- Network segmentation and firewall configuration
- Additional remote access controls and security hardening

### Linux and Containers

- Linux user and permission management
- Additional Docker Compose application deployments
- Container networking and storage

### Windows Server

- Windows Server virtual machine
- Active Directory Domain Services
- Group Policy
- Windows DNS and DHCP administration

### Storage and Backups

- Immich
- Nextcloud
- Syncthing
- SMB file sharing
- Proxmox Backup Server
- Independent backup storage and restore testing

### Media and Game Servers

- Jellyfin
- Additional AMP-managed game servers
- Additional Minecraft server configuration and optimization

### Monitoring

- Uptime Kuma
- Beszel
- Live service and infrastructure metrics

### Security

- Firewall configuration
- Access control
- Service hardening
- Secure remote administration

## Repository Structure

The following tree reflects files currently present in the repository. Planned documentation directories will be added as their corresponding project milestones are completed.

```text
OptiPlex-7050-Homelab/
│
├── README.md
├── CHANGELOG.md
├── LICENSE
│
├── 01-hardware/
│   ├── bios-configuration.md
│   ├── hardware-inventory.md
│   └── storage-health.md
│
├── 02-proxmox/
│   ├── configuration.md
│   ├── installation.md
│   └── vm-inventory.md
│
├── 03-networking/
│   ├── dns-dhcp.md
│   ├── network-overview.md
│   └── tailscale.md
│
├── 06-docker/
│   └── containers.md
│
├── 08-backups/
│   └── backup-strategy.md
│
└── 09-applications/
    ├── amp.md
    ├── minecraft-server.md
    └── playit.md
```

## Documentation Standards

This repository is intended to be an accurate technical record of the homelab.

- Document actual configurations and verified results.
- Distinguish completed implementations from planned projects.
- Record troubleshooting steps and lessons learned.
- Keep credentials, passwords, API tokens, MAC addresses, and device serial numbers out of public documentation.
- Do not publish public IP addresses or private authentication details.
- Use placeholders for private LAN addressing in public documentation.
- Avoid publishing private Tailscale addresses and actual public tunnel hostnames.
- Update the changelog when meaningful milestones are completed.
- Document significant configuration changes and infrastructure decisions.

## Learning Outcomes

This project provides practical experience with:

- Hardware inventory and troubleshooting
- BIOS and virtualization configuration
- Proxmox VE administration
- Virtual machine deployment and management
- Debian Linux administration
- SSH and user permissions
- Docker Engine and Docker Compose
- DNS and DHCP configuration
- Network troubleshooting
- Internal DNS architecture
- Tailscale private networking
- Tailscale Serve
- Public service tunneling with Playit.gg
- Application and game-server deployment
- Java runtime management
- Backup configuration and retention
- Technical documentation and change management

## Disclaimer

This is an independent personal learning project. Technologies and services listed under planned projects are not represented as completed professional or production deployments.

The purpose is to demonstrate hands-on learning, technical problem-solving, and infrastructure administration.

---

Project: OptiPlex 7050 Homelab  
Platform: Dell OptiPlex 7050 SFF  
Hypervisor: Proxmox VE  
Repository: [GitHub - OptiPlex-7050-Homelab](https://github.com/iSleepin/OptiPlex-7050-Homelab)


# Docker Container Inventory

## Overview

Docker Engine and Docker Compose are installed on the Debian 13 Services virtual machine hosted by Proxmox VE.

Docker is used to deploy and manage homelab applications in isolated containers.

## Docker Environment

| Component | Configuration |
|---|---|
| Operating System | Debian 13 |
| Docker Host | `debian-services` |
| Docker Version | 29.10.0 |
| Docker Compose Version | 5.8.0 |
| Deployment Method | Docker Compose |
| Container Storage | `/opt/` |

## Deployed Containers

### Technitium DNS Server

| Property | Configuration |
|---|---|
| Container Name | `technitium` |
| Image | `technitium/dns-server:latest` |
| Restart Policy | `unless-stopped` |
| DNS | TCP/UDP 53 |
| Web Administration | TCP 5380 |
| Deployment Directory | `/opt/technitium` |

#### Persistent Storage

| Host Directory | Container Directory | Purpose |
|---|---|---|
| `./config` | `/etc/dns` | DNS configuration |
| `./logs` | `/var/log/technitium/dns` | DNS logs |

#### Management Interface

`http://dns.home.arpa:5380`

#### Deployment Commands

Run these commands from `/opt/technitium`:

```bash
docker compose pull
docker compose up -d
docker compose ps
```

#### Management Commands

Check running containers:

```bash
docker ps
```

View Technitium logs:

```bash
docker logs technitium
```

Stop the deployment:

```bash
docker compose down
```

Start the deployment:

```bash
docker compose up -d
```

## Security Considerations

- Docker access is restricted to trusted administrators.
- The `homelab` account belongs to the Docker group, which provides root-equivalent privileges.
- Persistent configuration and logs are stored outside the container.
- Administrative credentials and secrets are not committed to GitHub.
- Network services are not intentionally exposed to the public internet.

## Future Docker Projects

- Immich
- Jellyfin
- Nextcloud
- Uptime Kuma
- Beszel
- Minecraft server management

These applications are planned and have not yet been deployed.
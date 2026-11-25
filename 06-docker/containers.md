
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

### Homepage Dashboard

| Property | Configuration |
|---|---|
| Container Name | `homepage` |
| Image | `ghcr.io/gethomepage/homepage:latest` |
| Restart Policy | `unless-stopped` |
| Web Interface | TCP 3000 |
| Deployment Directory | `/opt/homepage` |

#### Persistent Storage

| Host Directory | Container Directory | Purpose |
|---|---|---|
| `./config` | `/app/config` | Homepage configuration |

#### Management Interface

`http://<DEBIAN-SERVICES-LAN-IP>:3000`

Homepage provides a centralized dashboard with shortcuts to Proxmox VE, Technitium DNS, AMP, and the Minecraft server.

The dashboard also displays live Proxmox, Uptime Kuma, and Beszel information, including an overview of monitored systems and resource metrics for both Debian VMs.

#### Deployment Commands

Run these commands from `/opt/homepage`:

```bash
docker compose pull
docker compose up -d
docker compose ps
```

### Uptime Kuma

| Property | Configuration |
|---|---|
| Container Name | `uptime-kuma` |
| Image | `louislam/uptime-kuma:2` |
| Restart Policy | `unless-stopped` |
| Web Interface | TCP 3001 |
| Deployment Directory | `/opt/uptime-kuma` |
| Persistent Data | `./data` mounted at `/app/data` |

Uptime Kuma monitors five services: Proxmox VE, Homepage, the Technitium web interface, AMP, and the Minecraft Java server. The Minecraft check uses GameDig with the Minecraft Java Edition protocol.

The Technitium monitor checks its HTTP administration interface; it does not independently validate DNS query resolution. The Proxmox monitor ignores the TLS certificate error because the local Proxmox web interface uses a self-signed certificate.

Homepage displays the Uptime Kuma status-page widget. Detailed monitor configuration is documented in [`07-monitoring/monitoring.md`](../07-monitoring/monitoring.md).

### Beszel Hub and Debian Services Agent

| Property | Configuration |
|---|---|
| Hub Container | `beszel` |
| Image | `henrygd/beszel:latest` |
| Restart Policy | `unless-stopped` |
| Web Interface | TCP 8090 |
| Deployment Directory | `/opt/beszel` |
| Persistent Data | `./data` mounted at `/beszel_data` |
| Agent Container on VM 100 | `beszel-agent` |
| Agent Image | `henrygd/beszel-agent` |
| Agent Deployment Directory | `/opt/beszel-agent` |

The Beszel agent on VM 100 runs as a Docker container using host networking. It reports host resource metrics to the Beszel Hub. A separate standalone agent runs on VM 101 and is managed by systemd; its configuration is documented in [`07-monitoring/monitoring.md`](../07-monitoring/monitoring.md).

Beszel collects CPU, memory, disk, average load, network activity, and uptime metrics for both Debian VMs.

## Docker Startup Troubleshooting

### Issue: Containers Failed to Start After Host Reboot

**Symptoms**

After rebooting the Proxmox host, VM 100 started, but the Homepage and Technitium web interfaces were inaccessible.

The Docker containers had failed to start because Docker attempted to bind their published ports to the VM's reserved LAN address before Debian had assigned that address to its network interface.

The relevant Docker error was:

```text
cannot assign requested address
```

### Root Cause

The Docker Compose configurations bind published ports to a specific IPv4 address on the VM's `ens18` interface.

During boot, Docker started before the DHCP-assigned address was available. As a result, Docker could not bind the requested host ports and the affected containers exited.

### Initial Troubleshooting Attempt

The `ifupdown-wait-online.service` systemd service was enabled to make Docker wait for network initialization.

However, this did not resolve the issue. The wait service completed before the expected DHCP address was actually assigned to `ens18`.

This demonstrated that waiting for the network service to complete was insufficient for this configuration.

### Resolution: Docker systemd Startup Dependency

A systemd drop-in was created to make Docker wait until the expected IPv4 address appears on the VM's network interface.

File location:

```text
/etc/systemd/system/docker.service.d/wait-for-lan-ip.conf
```

Example configuration:

```ini
[Service]
ExecStartPre=/bin/sh -c 'for i in $(seq 1 120); do ip -4 -o addr show dev ens18 | grep -q "192.0.2.10/24" && exit 0; sleep 1; done; echo "Timed out waiting for the expected LAN address on ens18" >&2; exit 1'
```

**Configuration note:** `192.0.2.10/24` is a documentation-only placeholder from the TEST-NET-1 range. Replace it with the actual reserved IPv4 address and prefix length for the VM when adapting this configuration. The interface name must also match the target system.

The loop checks once per second and allows up to 120 seconds for the address to appear. If the address does not appear before the timeout, the pre-start command exits with an error instead of allowing Docker to start prematurely.

After creating the drop-in, systemd was reloaded:

```bash
sudo systemctl daemon-reload
```

The override was inspected using:

```bash
systemctl cat docker
systemctl show docker -p ExecStartPre
```

### Verification

The fix was tested by rebooting the Debian services VM.

The following checks passed:

- VM 100 received its expected LAN address.
- Docker started automatically after the address became available.
- Homepage started and reported a healthy container status.
- Technitium started successfully.
- Both web interfaces were reachable from a Windows client.
- No Docker host-port binding errors appeared in the filtered boot journal.

Useful verification commands:

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

Inspect Docker startup logs:

```bash
sudo journalctl -b -u docker --no-pager
```

From a Windows PowerShell client, test the published ports:

```powershell
Test-NetConnection <DEBIAN-SERVICES-LAN-IP> -Port 3000
Test-NetConnection <DEBIAN-SERVICES-LAN-IP> -Port 5380
```

Both tests returned `TcpTestSucceeded : True` after the reboot.

### Lessons Learned

- Docker restart policies do not resolve host-port binding failures caused by unavailable host addresses.
- `network-online.target` and a wait-online service do not necessarily guarantee that a DHCP-assigned address is present.
- Systemd service dependencies can be extended with drop-in configuration.
- Startup behavior should be verified through an actual reboot rather than relying only on successful manual container starts.

## Security Considerations

- Docker access is restricted to trusted administrators.
- The `homelab` account belongs to the Docker group, which provides root-equivalent privileges.
- Persistent configuration and logs are stored outside the container.
- Administrative credentials and secrets are not committed to GitHub.
- Network services are not intentionally exposed to the public internet.
- Administrative interfaces remain restricted to private network or authorized remote access.

## Future Docker Projects

- Immich
- Jellyfin
- Nextcloud

These applications are planned and have not yet been deployed.


# CubeCoders AMP — Game Server Management

## Overview

CubeCoders AMP (Application Management Panel) is used to manage and operate the homelab's Minecraft server.

AMP runs inside a dedicated Debian virtual machine on Proxmox VE. It provides a web-based interface for managing game server instances, configuring resources, monitoring server activity, and controlling server processes.

The Linggango modded Minecraft server runs as an AMP-managed instance.

## Deployment Environment

| Property | Configuration |
|---|---|
| Platform | Proxmox VE |
| VM ID | 101 |
| VM Hostname | `amp-server` |
| Operating System | Debian 13 |
| vCPU | 4 |
| Memory | 12 GB |
| Virtual Disk | 100 GB |
| Network Adapter | VirtIO |
| Management Software | CubeCoders AMP |
| AMP Version | 2.8.0.10 |

## AMP Installation

AMP was installed on the Debian 13 virtual machine using the CubeCoders installer.

The installation configured:

- AMP Instance Manager
- Standalone deployment
- Podman support
- Java runtime support
- SteamCMD dependencies
- AMP management services

The AMP installation includes the management instance `ADS01`.

## Management Access

The AMP management interface is available through the local network:

```text
http://<AMP-SERVER-LAN-IP>:8080
```

The AMP VM also has private Tailscale connectivity for remote administration.

AMP management is not published through Playit.gg.

Administrative access should remain restricted to trusted devices on the LAN or private Tailscale network.

## Instance Configuration

### Linggango01

The Minecraft server is managed through an AMP instance named `Linggango01`.

| Property | Configuration |
|---|---|
| AMP Instance | `Linggango01` |
| Application | Minecraft Java Edition |
| Server Type | Forge |
| Minecraft Version | 1.20.1 |
| Forge Version | 47.4.20 |
| Java Runtime | Eclipse Temurin 17 |
| Minecraft Port | 25565/TCP |
| Maximum Players | 5 |

The Minecraft server files are stored under:

```text
/home/amp/.ampdata/instances/Linggango01/Minecraft/
```

The instance uses the extracted Linggango server pack.

See `minecraft-server.md` for the complete server configuration.

## Resource Allocation

The AMP VM has 4 virtual CPUs and 12 GB of RAM.

The Minecraft instance is configured with:

| Setting | Value |
|---|---:|
| AMP Memory Limit | 8192 MB |
| Memory Pre-allocation | 6144 MB |

These settings reserve resources for the Minecraft process while leaving memory available for Debian and AMP.

Resource usage should be monitored during multiplayer sessions and adjusted as necessary.

## Remote Access Architecture

Two separate services provide connectivity to the homelab:

### Tailscale — Private Administration

Tailscale provides private remote access to the AMP VM.

The AMP management interface can be reached through its Tailscale IP and management port.

This access is intended for administration by trusted devices.

### Playit.gg — Public Minecraft Connectivity

Playit.gg provides public connectivity to the Minecraft server.

The Playit agent runs on the AMP VM and forwards connections to:

```text
127.0.0.1:25565
```

The public tunnel is dedicated to Minecraft Java connectivity.

AMP management, SSH, and other administrative services are not exposed through the Playit tunnel.

The actual public hostname is intentionally excluded from this repository.

See:

- `../03-networking/tailscale.md`
- `playit.md`
- `minecraft-server.md`

## Backup Configuration

AMP's backup functionality has been configured for the Minecraft instance.

Current retention settings:

| Setting | Value |
|---|---|
| Schedule | Configured as hourly; execution not yet verified |
| Retention Count | 24 |
| Backup Storage | `./Backups/` |
| Compression | Optimal |
| Replacement Policy | Delete Single Oldest |

The backup configuration has been established, but successful scheduled backups and restoration have not yet been verified.

See `../08-backups/backup-strategy.md`.

## Verification

The following functionality has been verified:

- AMP installed successfully on Debian 13.
- AMP management instance running.
- Minecraft instance created through AMP.
- Linggango server pack extracted into the instance directory.
- Minecraft server successfully started.
- Minecraft process listening on TCP port 25565.
- AMP reachable over the local network.
- AMP reachable over Tailscale.
- Playit agent connected successfully.
- Public Minecraft connectivity tested from outside the home network.

## Security Considerations

- Keep AMP management access private.
- Use Tailscale for remote administration.
- Use Playit.gg only for public Minecraft connectivity.
- Do not expose the AMP management port through public tunnels.
- Do not expose SSH or Proxmox management interfaces publicly.
- Keep AMP credentials and Playit agent secrets out of Git.
- Use strong authentication for administrative accounts.
- Keep Debian and AMP components updated.

## Maintenance

Check the AMP management service and instance status through the AMP interface.

Check the Playit agent:

```bash
sudo systemctl status playit --no-pager
```

Check Tailscale connectivity:

```bash
tailscale status
```

Monitor CPU and memory usage through AMP or Debian system utilities.

## Future Improvements

- Verify automatic backup creation.
- Perform a backup restoration test.
- Monitor server resource consumption.
- Review AMP instance update procedures.
- Document recovery procedures.
- Evaluate independent backup storage.

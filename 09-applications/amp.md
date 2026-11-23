# CubeCoders AMP Deployment

## Overview

CubeCoders AMP (Application Management Panel) is deployed in a dedicated Debian 13 virtual machine on Proxmox VE.

AMP provides a web-based interface for creating, managing, and monitoring game-server instances.

The deployment is intended to support Minecraft and other game servers while keeping game-server workloads separate from the homelab's Docker and DNS services.

## Virtual Machine

AMP runs on Proxmox VM 101.

| Property | Configuration |
|---|---|
| VM ID | 101 |
| Hostname | `amp-server` |
| Operating System | Debian 13 |
| Architecture | AMD64 |
| vCPU | 4 |
| CPU Type | Host |
| Memory | 12 GB |
| Virtual Disk | 100 GB |
| Network Adapter | VirtIO |
| IP Address | `192.168.68.104` |
| Default Gateway | `192.168.68.1` |

## AMP Installation

AMP was installed using the official CubeCoders installation script.

The installation was performed from a root shell on the Debian VM.

```bash
sudo -i
bash <(curl -fsSL getamp.sh)
```

The installer detected Debian 13 and installed AMP Instance Manager.

### Installation Options

| Option | Configuration |
|---|---|
| Instance Manager | Installed |
| Management Instance | `ADS01` |
| Management Port | 8080 |
| Podman | Installed |
| Java runtimes | Installed |
| SteamCMD 32-bit libraries | Installed |
| HTTPS / Nginx | Not enabled |
| Firewall package | Not detected by installer |

HTTPS/Nginx was not enabled because the environment is currently intended for LAN access and does not use a public domain for AMP.

No router port forwarding was configured for AMP.

## AMP Configuration

### Operation Mode

AMP is configured in **Standalone** mode.

This mode is suitable for the current single-host deployment and allows the Instance Manager to manage local game-server instances.

### Privacy Settings

| Setting | Configuration |
|---|---|
| Anonymous crash reports | Enabled |
| Anonymous usage data | Disabled |
| Enhanced licence reporting | Disabled |

### Management Interface

AMP web interface:

`http://192.168.68.104:8080`

The AMP administrative username and password are intentionally excluded from this repository.

## Installed Components

The installation includes:

- AMP Instance Manager
- Podman support for instance isolation
- Java runtimes for Java-based game servers
- SteamCMD 32-bit libraries for future game-server support

The Minecraft instance uses Eclipse Temurin 17.

## Current Instances

| Instance | Application | Status |
|---|---|---|
| Minecraft | Linggango modded Minecraft server | Running |

Additional game-server instances may be created through AMP as resources permit.

## Operational Notes

- Use the AMP web interface to manage instances and monitor their consoles.
- Use SSH for Debian system administration.
- Keep game-server files inside their assigned AMP instance directories.
- Do not manually run server startup scripts when AMP is managing the instance.
- Monitor CPU, memory, and disk utilization.
- Keep administrative interfaces restricted to trusted network access.

## Installation Notes and Warnings

During installation, the AMPThemes Git repository fetch reported an error while downloading theme files.

The AMP Instance Manager nevertheless completed installation, created the `ADS01` management instance, and became accessible through its web interface.

The theme repository warning has not yet been separately investigated.

The installer also reported that no firewall package was detected. Proxmox VM firewall functionality is enabled for VM 101, but custom firewall rules have not been documented as verified.

## Security Considerations

- AMP is currently accessed through the local network.
- No public router port forwarding has been configured.
- HTTPS/Nginx was not enabled.
- Administrative credentials are not stored in this repository.
- Remote access through Tailscale is a future project.

## Future Improvements

- Verify firewall rules and restrict management access.
- Configure and test automatic backups.
- Establish independent backup storage.
- Add additional AMP-managed game servers if resources permit.
- Document resource utilization and performance.
- Configure secure remote access.
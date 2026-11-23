# Proxmox Virtual Machine Inventory

## Overview

This document tracks virtual machines deployed on the Proxmox VE host running on the Dell OptiPlex 7050 SFF.

The environment currently uses two Debian 13 virtual machines with separate responsibilities:

- VM 100: Docker and network services.
- VM 101: AMP and game-server management.

## Host Information

| Property | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| Hostname | `pve` |
| Management IP | `<PROXMOX-LAN-IP>` |
| CPU | Intel Core i7-7700 |
| Physical Cores | 4 |
| Logical Threads | 8 |
| Physical Memory | 32 GB |
| Storage | 1 TB SATA SSD |

## Virtual Machine Inventory

| VM ID | Name | Operating System | vCPU | RAM | Disk | IP Address | Purpose |
|---|---|---|---:|---:|---:|---|---|
| 100 | `debian-services` | Debian 13 | 2 | 4 GB | 32 GB | `<DEBIAN-SERVICES-LAN-IP>` | Docker and network services |
| 101 | `amp-server` | Debian 13 | 4 | 12 GB | 100 GB | `<AMP-SERVER-LAN-IP>` | AMP and game servers |

## VM 100 - Debian Services

### Configuration

| Property | Value |
|---|---|
| VM ID | 100 |
| Hostname | `debian-services` |
| Operating System | Debian 13 |
| vCPU | 2 |
| Memory | 4 GB |
| Virtual Disk | 32 GB |
| Network Adapter | VirtIO |
| IP Address | `<DEBIAN-SERVICES-LAN-IP>` |
| Default Gateway | `<LAN-GATEWAY-IP>` |
| Administration | SSH |
| QEMU Guest Agent | Installed and running |

### Responsibilities

VM 100 hosts network and Docker services.

Current services include:

- Docker Engine
- Docker Compose
- Technitium DNS Server

### Management

SSH access:

```bash
ssh homelab@<DEBIAN-SERVICES-LAN-IP>
```

Technitium administration:

`http://dns.home.arpa:5380`

## VM 101 - AMP Server

### Configuration

| Property | Value |
|---|---|
| VM ID | 101 |
| Hostname | `amp-server` |
| Operating System | Debian 13 |
| Architecture | AMD64 |
| vCPU | 4 |
| CPU Type | Host |
| Memory | 12 GB |
| Memory Ballooning | Disabled |
| Virtual Disk | 100 GB |
| Network Adapter | VirtIO |
| IP Address | `<AMP-SERVER-LAN-IP>` |
| Default Gateway | `<LAN-GATEWAY-IP>` |
| BIOS | SeaBIOS |
| Machine Type | i440fx |
| SCSI Controller | VirtIO SCSI Single |
| QEMU Guest Agent | Installed and running |

### Responsibilities

VM 101 is dedicated to CubeCoders AMP and game-server management.

AMP manages game-server instances within this virtual machine. This keeps game-server workloads separate from the Docker and DNS services running on VM 100.

### Installed Components

- Debian 13
- CubeCoders AMP Instance Manager
- Podman support
- Java runtimes
- SteamCMD 32-bit libraries

### Management

SSH access:

```bash
ssh homelab@<AMP-SERVER-LAN-IP>
```

AMP web interface:

`http://<AMP-SERVER-LAN-IP>:8080`

AMP is configured in Standalone mode. Its management instance is named `ADS01`.

The VM is also connected to the private Tailscale network for remote administration.

### Current Game Server

- Game: Minecraft Java Edition
- Modpack: Linggango
- Minecraft version: 1.20.1
- Forge version: 47.4.20
- Java runtime: Eclipse Temurin 17

Additional details are documented in `09-applications/minecraft-server.md`.

## Network Architecture

Both virtual machines use the Proxmox virtual bridge and receive addresses on the home LAN.

| Component | Address |
|---|---|
| Proxmox VE | `<PROXMOX-LAN-IP>` |
| Debian Services VM | `<DEBIAN-SERVICES-LAN-IP>` |
| AMP Server VM | `<AMP-SERVER-LAN-IP>` |
| Default Gateway | `<LAN-GATEWAY-IP>` |

The TP-Link Deco M9 Plus provides DHCP reservations for the virtual machines.

## Resource Allocation Considerations

The OptiPlex 7050 has 32 GB of physical RAM and 8 logical CPU threads.

Current allocations:

| Resource | VM 100 | VM 101 | Total |
|---|---:|---:|---:|
| vCPU | 2 | 4 | 6 |
| RAM | 4 GB | 12 GB | 16 GB |
| Virtual Disk | 32 GB | 100 GB | 132 GB |

These are configured virtual resources, not a measurement of actual consumption.

The remaining host resources are reserved for Proxmox overhead and future workloads.

## Operational Notes

- Keep application workloads inside their assigned VMs rather than installing them directly on the Proxmox host.
- Use SSH for Debian administration.
- Use the Proxmox web interface for VM hardware and lifecycle management.
- Monitor memory and storage utilization as additional services are deployed.
- Keep backups separate from the primary VM storage when possible.
- Do not publish passwords, private keys, MAC addresses, or authentication tokens.

## Future Improvements

- Deploy Proxmox Backup Server or another independent backup target.
- Implement resource monitoring.
- Document VM snapshots and backup procedures.
- Review CPU and memory allocations as workloads increase.
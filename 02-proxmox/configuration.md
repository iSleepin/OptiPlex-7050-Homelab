
# Proxmox VE Initial Configuration

## Host Information

| Setting | Value |
|---|---|
| Hypervisor | Proxmox VE |
| Node Name | `pve` |
| Management IP | `<PROXMOX-LAN-IP>` |
| Subnet | `/24` |
| Gateway | `<LAN-GATEWAY-IP>` |
| DNS Server | `<LAN-GATEWAY-IP>` |
| Web Interface | `https://<PROXMOX-LAN-IP>:8006` |

## System Resources

| Resource | Reported Value |
|---|---|
| Processor | Intel Core i7-7700 |
| CPU Threads | 8 |
| Memory | 31.29 GiB |
| Local Storage | Approximately 93.93 GiB |
| Local-LVM Storage | Approximately 875.49 GB |

The reported memory is slightly below the installed 32 GB because some memory is reserved for system hardware and the operating environment.

## Storage Configuration

The installation uses the default LVM-based storage layout with ext4.

### local

- Type: Directory
- Approximate capacity: 93.93 GiB
- Intended for ISO images, templates, backups, and other supported content.

### local-lvm

- Type: LVM-thin
- Approximate capacity: 875.49 GB
- Intended for virtual machine disks and container storage.

## Network Configuration

The Proxmox host is connected to the home network through the Intel I219-LM Gigabit Ethernet adapter.

| Setting | Value |
|---|---|
| Network Interface | `nic0` |
| Router | TP-Link Deco M9 Plus |
| Gateway | `<LAN-GATEWAY-IP>` |
| Proxmox IP | `<PROXMOX-LAN-IP>/24` |
| DNS | `<LAN-GATEWAY-IP>` |

The host IP address is reserved in the Deco router's address reservation configuration.

## Connectivity Testing

### Local Gateway

```bash
GATEWAY_IP="<LAN-GATEWAY-IP>"  # Replace the placeholder before running
ping -c 4 "$GATEWAY_IP"
```

Result: Successful, 0% packet loss.

### DNS and Internet

```bash
ping -c 4 google.com
```

Result: Successful DNS resolution and connectivity, 0% packet loss.

## Management Access

The Proxmox web interface is accessible from another device on the local network:

`https://<PROXMOX-LAN-IP>:8006`

Administrative access uses the root account configured during installation.

Credentials are intentionally excluded from this repository.

## Operational Notes

- The Proxmox host is dedicated to virtualization and infrastructure management.
- Application workloads should run inside virtual machines or containers rather than directly on the Proxmox host.
- VM resource allocations should account for the host's available CPU and memory.
- Backups should eventually be stored separately from the primary SSD.
- Remote administrative access should be secured rather than exposing the management interface directly to the public internet.

## Next Steps

- Review Proxmox network configuration.
- Plan VM network addresses.
- Download a Debian installation ISO.
- Create and configure the first Linux virtual machine.

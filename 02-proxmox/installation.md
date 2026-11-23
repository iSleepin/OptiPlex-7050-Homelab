
# Proxmox VE Installation

## Overview

Proxmox Virtual Environment (Proxmox VE) was installed directly onto the Dell OptiPlex 7050 SFF to serve as the homelab's bare-metal hypervisor.

The installation replaced the previous Windows installation.

## Installation Hardware

| Component | Specification |
|---|---|
| System | Dell OptiPlex 7050 SFF |
| Processor | Intel Core i7-7700 |
| CPU Threads | 8 |
| RAM | 32 GB DDR4 |
| Target Disk | 1 TB KingFast SATA SSD |

## Installation Preparation

- Downloaded the official Proxmox VE ISO.
- Downloaded Rufus to create a bootable USB drive.
- Created the bootable USB installer.
- Verified hardware virtualization support in BIOS.
- Confirmed existing data could be erased.

## Installation Configuration

| Setting | Configuration |
|---|---|
| Installation Type | Bare-metal |
| Target Disk | KingFast 1 TB SATA SSD |
| Filesystem | ext4 |
| Storage Layout | Default LVM and LVM-thin |
| Hostname | `pve` |
| Management Interface | `nic0` |
| Management IP | `<PROXMOX-LAN-IP>/24` |
| Gateway | `<LAN-GATEWAY-IP>` |
| DNS Server | `<LAN-GATEWAY-IP>` |
| Region | United States |
| Time Zone | America/Los_Angeles |
| Keyboard Layout | en-US |

## Network Environment

The homelab is connected to a TP-Link Deco M9 Plus mesh network operating in Router Mode.

| Network Setting | Value |
|---|---|
| Router / Gateway | `<LAN-GATEWAY-IP>` |
| Subnet Mask | `255.255.255.0` |
| Network | `<LAN-SUBNET>` |
| DHCP Server | `<LAN-GATEWAY-IP>` |
| DHCP Range | `<DHCP-POOL-START-IP>` – `<DHCP-POOL-END-IP>` |
| Proxmox Reserved IP | `<PROXMOX-LAN-IP>` |

The Proxmox host address was reserved in the router's address reservation settings.

## Installation Result

Proxmox VE installed successfully and the system rebooted into the Proxmox environment.

The web management interface was accessible at:

`https://<PROXMOX-LAN-IP>:8006`

The initial subscription notification was dismissed. The installation is being used without a paid subscription.

## Post-Installation Verification

The following checks were completed from the Proxmox host shell.

### Gateway Connectivity

Command:

```bash
GATEWAY_IP="<LAN-GATEWAY-IP>"  # Replace the placeholder before running
ping -c 4 "$GATEWAY_IP"
```

Result:
- 4 packets transmitted
- 4 packets received
- 0% packet loss

### Internet and DNS Connectivity

Command:

```bash
ping -c 4 google.com
```

Result:
- DNS resolution succeeded.
- 4 packets transmitted
- 4 packets received
- 0% packet loss

### Web Interface

- Proxmox web interface opened successfully.
- Node dashboard displayed system information.
- Local and local-lvm storage were visible.

## Result

Proxmox VE is installed and operational.

The host can communicate with the local gateway and access the internet through DNS resolution.

## Next Steps

- Configure and verify virtual networking.
- Plan virtual machine IP assignments.
- Deploy a Linux virtual machine.
- Begin Linux administration and network services.

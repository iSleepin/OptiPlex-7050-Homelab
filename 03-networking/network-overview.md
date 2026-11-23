# Homelab Network Overview

## Network Architecture

The homelab uses a TP-Link Deco M9 Plus mesh router as its gateway and DHCP server. Proxmox VE hosts the virtual machines, while a Debian VM provides Docker-based network services.

## Network Topology

```text
                         Internet
                            |
                            |
                  TP-Link Deco M9 Plus
                    192.168.68.1
                    Router / DHCP
                            |
                      Local Network
                     192.168.68.0/24
                            |
             +--------------+--------------+
             |                             |
       Windows Client                 Proxmox VE
       192.168.68.110                 192.168.68.200
                                           |
                                      Virtual Bridge
                                           |
                                    Debian Services VM
                                      192.168.68.108
                                           |
                                      Docker Engine
                                           |
                                   Technitium DNS
                                      TCP/UDP 53
                                      TCP 5380
                                           |
                                    Cloudflare DoH
```

## Network Address Plan

| Device / Service | IP Address | Purpose |
|---|---|---|
| TP-Link Deco M9 Plus | `192.168.68.1` | Default gateway and DHCP |
| Proxmox VE | `192.168.68.200` | Hypervisor |
| Debian Services VM | `192.168.68.108` | Linux and Docker host |
| Windows Client | `192.168.68.110` | Administration workstation |

## Infrastructure Details

### Router

- Model: TP-Link Deco M9 Plus
- Operating mode: Router Mode
- Gateway: `192.168.68.1`
- Subnet: `192.168.68.0/24`
- DHCP server: Enabled
- DHCP pool: `192.168.68.100`–`192.168.68.250`

### Hypervisor

- Platform: Proxmox VE
- Host IP: `192.168.68.200`
- Management interface: TCP port `8006`
- Virtual network bridge: `vmbr0`

### Debian Services Virtual Machine

- VM ID: `100`
- Guest OS: Debian 13
- Hostname: `debian-services`
- IP address: `192.168.68.108`
- vCPU allocation: 2
- Memory allocation: 4 GB
- Virtual disk: 32 GB
- Container platform: Docker Engine and Docker Compose

### DNS Infrastructure

- DNS platform: Technitium DNS Server
- Deployment: Docker Compose
- Internal zone: `home.arpa`
- Upstream DNS: Cloudflare DNS-over-HTTPS
- DNS ports: TCP/UDP 53

## Internal Service Naming

| Hostname | Service |
|---|---|
| `proxmox.home.arpa` | Proxmox VE |
| `dns.home.arpa` | Technitium DNS |

## Network Design Decisions

- The Deco router remains responsible for DHCP.
- Technitium handles DNS resolution for DHCP clients.
- Docker services run inside the Debian VM rather than directly on the Proxmox host.
- The `home.arpa` namespace is used for internal DNS.
- Infrastructure services use reserved or assigned IP addresses to maintain consistent access.

## Future Network Improvements

- Configure Tailscale for secure remote administration.
- Add DNS records as additional services are deployed.
- Develop a more detailed network diagram as the environment grows.
- Evaluate network segmentation and firewall policies.
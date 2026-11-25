# Homelab Network Overview

## Network Architecture

The homelab uses a TP-Link Deco M9 Plus mesh router in Router Mode for routing and DHCP. Proxmox VE runs on a Dell OptiPlex 7050 SFF and hosts two Debian virtual machines.

The Debian services VM provides Docker-based DNS and dashboard services. A separate Debian VM runs CubeCoders AMP and the Linggango Minecraft server.

Private LAN addresses are represented by placeholders in this public repository.

## Network Topology

```text
                                  Internet
                                     |
                            TP-Link Deco M9 Plus
                              Router / DHCP
                           <LAN-GATEWAY-IP>
                                     |
                              Local Network
                               <LAN-SUBNET>
                                     |
              +----------------------+---------------------+
              |                                            |
       Windows Admin PC                              Proxmox VE
        Tailscale client                           <PROXMOX-LAN-IP>
                                                         |
                                                   Virtual Bridge
                                                      vmbr0
                                                         |
                            +----------------------------+---------------------+
                            |                                                  |
                    VM 100: debian-services                            VM 101: amp-server
                    <DEBIAN-SERVICES-LAN-IP>                            <AMP-SERVER-LAN-IP>
                            |                                                  |
                     Docker Engine                                      CubeCoders AMP
                       /        \                                           TCP 8080
                      /          \
             Technitium DNS     Homepage                                Minecraft Java
              TCP/UDP 53         TCP 3000                                TCP 25565
              TCP 5380                                                       |
                    |                                                  Playit agent
             Tailscale Serve                                           127.0.0.1:25565
             Tailnet-only HTTPS                                              |
                                                                       Public Minecraft
                                                                         tunnel only
```

Tailscale is installed on the Proxmox host, both Debian VMs, and the Windows administration computer. It provides private remote administration without exposing management interfaces directly to the internet.

Playit.gg provides public connectivity exclusively to the Minecraft server. The Playit agent forwards traffic to the local Minecraft listener at `127.0.0.1:25565`. No router port forwarding is configured for these services.

## Network Address Plan

| Device / Service | Address | Purpose |
|---|---|---|
| TP-Link Deco M9 Plus | `<LAN-GATEWAY-IP>` | Default gateway and DHCP |
| Proxmox VE | `<PROXMOX-LAN-IP>` | Hypervisor |
| Debian Services VM | `<DEBIAN-SERVICES-LAN-IP>` | Linux and Docker host |
| AMP Server VM | `<AMP-SERVER-LAN-IP>` | AMP and game server host |
| Windows administration PC | `<ADMIN-WORKSTATION-LAN-IP>` | Administration workstation |

The Deco router maintains the DHCP pool and reservations. Actual private addresses and the exact DHCP range are intentionally omitted from this public document.

## Infrastructure Details

### Router

- Model: TP-Link Deco M9 Plus
- Operating mode: Router Mode
- DHCP server: Enabled
- DHCP reservations: Configured for the Proxmox host and both VMs

### Hypervisor

- Platform: Proxmox VE
- Hostname: `pve`
- Management interface: TCP port `8006`
- Virtual network bridge: `vmbr0`

### Debian Services Virtual Machine

- VM ID: `100`
- Guest OS: Debian 13
- Hostname: `debian-services`
- vCPU allocation: 2
- Memory allocation: 4 GB
- Virtual disk: 32 GB
- Container platform: Docker Engine and Docker Compose

Services hosted on VM 100:

- Technitium DNS Server (TCP/UDP 53 and web administration TCP 5380)
- Homepage dashboard (TCP 3000)
- Uptime Kuma service monitoring (TCP 3001)
- Beszel Hub and Docker-based Beszel agent (TCP 8090 for the Hub; agent metrics collection)
- Tailscale Serve for private HTTPS access to Technitium

### AMP Virtual Machine

- VM ID: `101`
- Guest OS: Debian 13
- Hostname: `amp-server`
- vCPU allocation: 4
- Memory allocation: 12 GB
- Virtual disk: 100 GB

Services hosted on VM 101:

- CubeCoders AMP management (TCP 8080)
- Linggango Minecraft Java server (TCP 25565)
- Playit agent for public Minecraft connectivity
- Beszel standalone agent managed by systemd (TCP 45876 for agent collection)

## Internal Service Naming

The internal DNS zone is `home.arpa`.

| Hostname | Service |
|---|---|
| `proxmox.home.arpa` | Proxmox VE |
| `dns.home.arpa` | Technitium DNS |

## Remote Access Design

| Access Method | Purpose | Exposure |
|---|---|---|
| Tailscale | Private administration of Proxmox, Debian VMs, AMP, and Technitium | Tailnet only |
| Tailscale Serve | Private HTTPS access to Technitium web administration | Tailnet only |
| Playit.gg | Public Minecraft Java connectivity | Minecraft tunnel only |

Administrative services are not published through Playit.gg. Tailscale Funnel is not enabled.

## Network Design Decisions

- The Deco router remains responsible for routing and DHCP.
- Technitium handles DNS resolution for clients configured to use it.
- Docker services run inside the Debian services VM rather than directly on the Proxmox host.
- The `home.arpa` namespace is used for internal DNS.
- Infrastructure devices use DHCP reservations for consistent private addressing.
- Tailscale is used for private remote administration.
- Playit.gg is used only for public Minecraft connectivity.

## Future Network Improvements

- Evaluate network segmentation and firewall policies.
- Add internal DNS records as additional services are deployed.
- Develop a more detailed network diagram as the environment grows.
- Review DNS filtering behavior across clients and encrypted-DNS configurations.

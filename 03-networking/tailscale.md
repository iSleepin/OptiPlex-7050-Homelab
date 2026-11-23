# Tailscale — Private Remote Access

## Overview

Tailscale provides private remote access to homelab systems through a tailnet. It allows administrative services to remain reachable from authorized devices without exposing management interfaces directly to the public internet or configuring router port forwarding.

Tailscale is installed on:

- Proxmox host (`pve`)
- Debian services VM (`debian-services`)
- AMP VM (`amp-server`)
- Windows administration computer

Actual Tailscale IP addresses, tailnet identifiers, and MagicDNS hostnames are intentionally excluded from this public repository.

## Deployment Inventory

| Device | Hostname | Address |
|---|---|---|
| Proxmox host | `pve` | `<PROXMOX-TAILSCALE-IP>` |
| Debian services VM | `debian-services` | `<DEBIAN-SERVICES-TAILSCALE-IP>` |
| AMP VM | `amp-server` | `<AMP-SERVER-TAILSCALE-IP>` |
| Windows administration PC | Windows administration PC | `<WINDOWS-ADMIN-TAILSCALE-IP>` |

The placeholders identify the address type only. They are not actual connection addresses.

## Installation

Tailscale was installed on the Debian-based VMs and Proxmox host using the official installation instructions. The Windows administration computer uses the Tailscale application.

Each device was authenticated and connected to the same tailnet.

## Remote Administration

The following services were verified over Tailscale:

| Service | Address | Port |
|---|---|---:|
| Proxmox web interface | `<PROXMOX-TAILSCALE-IP>` | 8006 |
| AMP management | `<AMP-SERVER-TAILSCALE-IP>` | 8080 |
| Technitium DNS | `<DEBIAN-SERVICES-MAGICDNS-HOSTNAME>` | 443 |

The Technitium web interface uses Tailscale Serve to provide private HTTPS access to the locally hosted Docker container.

## Tailscale Serve

Tailscale Serve is configured on `debian-services` to proxy HTTPS traffic to Technitium DNS.

Configuration:

```bash
sudo tailscale serve --bg 5380
```

The service proxies requests to:

```text
http://127.0.0.1:5380
```

The resulting HTTPS endpoint is accessible only within the tailnet. Tailscale Funnel is not enabled.

## Verification

Connectivity was tested from the Windows administration computer using PowerShell.

Replace the following example values with the assigned tailnet addresses or MagicDNS hostname before running:

```powershell
$ProxmoxTailnetIP = "<PROXMOX-TAILSCALE-IP>"
$AmpTailnetIP = "<AMP-SERVER-TAILSCALE-IP>"
$TechnitiumMagicDNS = "<DEBIAN-SERVICES-MAGICDNS-HOSTNAME>"

Test-NetConnection $ProxmoxTailnetIP -Port 8006
Test-NetConnection $AmpTailnetIP -Port 8080
Test-NetConnection $TechnitiumMagicDNS -Port 443
```

All three connectivity tests returned successful TCP connections during verification.

## Security Considerations

- Administrative interfaces are accessed through the private tailnet.
- No router port forwarding is required for private administration.
- Tailscale Funnel is not enabled.
- Public Minecraft connectivity is handled separately through Playit.gg.
- Tailscale authentication credentials and authorization keys must never be committed to Git.
- Only trusted devices and users should have access to the tailnet.
- Actual tailnet IP addresses, MagicDNS hostnames, and tailnet identifiers are excluded from public documentation.

## Maintenance

Check Tailscale connectivity:

```bash
tailscale status
```

Check the local Tailscale IP:

```bash
tailscale ip -4
```

Check Tailscale Serve configuration:

```bash
sudo tailscale serve status
```

## Notes

Tailscale provides private administrative connectivity, while Playit.gg provides public connectivity for the Minecraft server. These services have separate roles in the homelab network architecture.

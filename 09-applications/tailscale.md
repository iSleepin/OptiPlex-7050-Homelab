
# Tailscale — Private Remote Access

## Overview

Tailscale provides secure remote access to homelab systems through a private network (tailnet).

It allows administrative services to remain accessible remotely without exposing management interfaces directly to the public internet or configuring router port forwarding.

Tailscale is installed on the Proxmox host, Debian services VM, AMP VM, and Windows administration computer.

## Deployment Inventory

| Device | Hostname | Tailscale IP |
|---|---|---|
| Proxmox host | `pve` | `100.118.228.85` |
| Debian services VM | `debian-services` | `100.90.151.88` |
| AMP VM | `amp-server` | `100.110.253.20` |
| Windows administration PC | `capybara-control-center` | `100.117.153.87` |

These are private Tailscale addresses and are documented for internal administration reference.

## Installation

Tailscale was installed on the Debian-based VMs and Proxmox host using the official installation script.

The Windows administration computer uses the Tailscale application.

Each device was authenticated and connected to the same tailnet.

## Remote Administration

The following services were verified over Tailscale:

| Service | Access Address | Port |
|---|---|---|
| Proxmox web interface | `100.118.228.85` | `8006` |
| AMP management | `100.110.253.20` | `8080` |
| Technitium DNS | `https://debian-services.tailc64cd9.ts.net` | `443` |

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

The resulting HTTPS endpoint is accessible only within the tailnet.

Tailscale Funnel is not enabled.

## Verification

Connectivity was tested from the Windows administration computer using PowerShell.

Examples:

```powershell
Test-NetConnection 100.118.228.85 -Port 8006
Test-NetConnection 100.110.253.20 -Port 8080
Test-NetConnection debian-services.tailc64cd9.ts.net -Port 443
```

All three connectivity tests returned successful TCP connections.

## Security Considerations

- Administrative interfaces are accessed through the private tailnet.
- No router port forwarding is required.
- Tailscale Funnel is not enabled.
- Public Minecraft connectivity is handled separately through Playit.gg.
- Tailscale authentication credentials and authorization keys must never be committed to Git.
- Only trusted devices and users should have access to the tailnet.

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

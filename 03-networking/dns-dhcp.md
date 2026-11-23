# DNS and DHCP Configuration

## Overview

This homelab uses a TP-Link Deco M9 Plus router for DHCP and Technitium DNS Server for internal and external DNS resolution.

The DNS server runs in a Docker container on a Debian 13 virtual machine hosted by Proxmox VE.

## Network Configuration

| Component | Configuration |
|---|---|
| Network | `192.168.68.0/24` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.68.1` |
| DHCP Server | TP-Link Deco M9 Plus |
| DNS Server | Technitium DNS |
| DNS Server IP | `192.168.68.108` |
| DNS Domain | `home.arpa` |

## DNS Server Deployment

Technitium DNS Server was deployed using Docker Compose on Debian 13.

- Container name: `technitium`
- Docker image: `technitium/dns-server:latest`
- DNS: TCP/UDP port 53
- Web Administration: TCP port 5380
- Persistent configuration: `/opt/technitium/config`
- Persistent logs: `/opt/technitium/logs`
- Restart policy: `unless-stopped`

### Upstream DNS

Configured Cloudflare DNS-over-HTTPS (DoH):

- Primary resolver: `1.1.1.1`
- Secondary resolver: `1.0.0.1`
- DoH endpoint: `https://cloudflare-dns.com/dns-query`

## DHCP Configuration

The TP-Link Deco M9 Plus remains responsible for DHCP.

The DHCP server distributes the Technitium server IP address (`192.168.68.108`) as the primary DNS server to network clients.

Technitium DHCP functionality remains unused.

## Internal DNS Zone

Created a primary DNS zone named `home.arpa`.

### DNS Records

| Record Type | Hostname | IP Address |
|---|---|---|
| A | `proxmox.home.arpa` | `192.168.68.200` |
| A | `dns.home.arpa` | `192.168.68.108` |

### Service Access

| Service | URL |
|---|---|
| Proxmox VE | `https://proxmox.home.arpa:8006` |
| Technitium DNS | `http://dns.home.arpa:5380` |

## Verification and Testing

The following tests were completed successfully:

- [x] External DNS resolution using Technitium over UDP.
- [x] External DNS resolution using Technitium over TCP.
- [x] Windows client DNS resolution through Technitium.
- [x] Deco DHCP distribution of Technitium DNS address.
- [x] Internal DNS resolution for Proxmox.
- [x] Internal DNS resolution for Technitium.
- [x] Browser access to Proxmox using its internal hostname.
- [x] Browser access to Technitium using its internal hostname.

### Example Verification Commands

Test external DNS:

```bash
dig @192.168.68.108 google.com
dig @192.168.68.108 google.com +tcp
```

Test internal DNS from Windows:

```powershell
nslookup proxmox.home.arpa
nslookup dns.home.arpa
```

## Security Considerations

- DNS administration is accessible through the local network.
- DNS forwarding uses encrypted DNS-over-HTTPS.
- DHCP remains managed by the router.
- Administrative credentials are not stored in this repository.
- Internal services are not intentionally exposed to the public internet.

## Future Improvements

- Add DNS records as additional homelab services are deployed.
- Configure Tailscale for secure remote access.
- Evaluate DNS filtering and blocklists.
- Consider HTTPS certificates for internal web services.
# DNS and DHCP Configuration

## Overview

The homelab uses a TP-Link Deco M9 Plus router for routing and DHCP, with Technitium DNS Server providing internal and external DNS resolution.

Technitium runs in a Docker container on the Debian 13 services VM hosted by Proxmox VE.

## Network Configuration

| Component | Configuration |
|---|---|
| Network | `<LAN-SUBNET>` |
| Subnet Mask | `<SUBNET-MASK>` |
| Default Gateway | `<LAN-GATEWAY-IP>` |
| DHCP Server | TP-Link Deco M9 Plus |
| DNS Server | Technitium DNS |
| DNS Server IP | `<DEBIAN-SERVICES-LAN-IP>` |
| DNS Domain | `home.arpa` |

Private LAN addresses are represented by placeholders in this public repository.

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

### DNS Filtering

Technitium's block-list feature is enabled with the **HaGeZi Multi Normal** list.

- Last recorded imported block-list size: approximately 177,323 entries
- Automatic list update interval: 24 hours
- Filtering is performed by Technitium for clients that use it for DNS resolution.

DNS filtering coverage is not guaranteed for every client. Devices using encrypted DNS, VPN DNS, or privacy features such as Apple Private Relay may bypass the network DNS resolver.

## DHCP Configuration

The TP-Link Deco M9 Plus remains responsible for DHCP.

The router distributes the Technitium server address (`<DEBIAN-SERVICES-LAN-IP>`) as the primary DNS server to network clients. The router also provides DHCP reservations for infrastructure devices.

Technitium DHCP functionality remains unused.

## Internal DNS Zone

A primary DNS zone named `home.arpa` was created.

### DNS Records

| Record Type | Hostname | IP Address |
|---|---|---|
| A | `proxmox.home.arpa` | `<PROXMOX-LAN-IP>` |
| A | `dns.home.arpa` | `<DEBIAN-SERVICES-LAN-IP>` |

### Service Access

| Service | URL |
|---|---|
| Proxmox VE | `https://proxmox.home.arpa:8006` |
| Technitium DNS | `http://dns.home.arpa:5380` |
| Homepage | `http://<DEBIAN-SERVICES-LAN-IP>:3000` |

## Verification and Testing

The following tests were completed successfully:

- [x] External DNS resolution using Technitium over UDP.
- [x] External DNS resolution using Technitium over TCP.
- [x] Windows client DNS resolution through Technitium.
- [x] Deco DHCP distribution of the Technitium DNS address.
- [x] Internal DNS resolution for Proxmox.
- [x] Internal DNS resolution for Technitium.
- [x] Browser access to Proxmox using its internal hostname.
- [x] Browser access to Technitium using its internal hostname.

### Example Verification Commands

Replace the placeholder with the actual DNS server address on your private network before running:

```bash
DNS_SERVER_IP="<DEBIAN-SERVICES-LAN-IP>"
dig @"${DNS_SERVER_IP}" google.com
dig @"${DNS_SERVER_IP}" google.com +tcp
```

Test internal DNS from Windows:

```powershell
nslookup proxmox.home.arpa <DEBIAN-SERVICES-LAN-IP>
nslookup dns.home.arpa <DEBIAN-SERVICES-LAN-IP>
```

## Security Considerations

- DNS administration is available on the LAN and through authorized private remote access.
- DNS forwarding uses encrypted DNS-over-HTTPS.
- DHCP remains managed by the router.
- Administrative credentials are not stored in this repository.
- Internal services are not intentionally exposed to the public internet.
- Client-side encrypted DNS and privacy services may bypass network-level DNS filtering.

## Future Improvements

- Add internal DNS records as additional homelab services are deployed.
- Validate DNS filtering behavior across different client devices.
- Consider HTTPS certificates for additional internal web services.
- Review filtering exceptions and false positives periodically.

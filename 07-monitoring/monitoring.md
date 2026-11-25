# Monitoring and Observability

## Overview

The homelab uses two complementary monitoring applications:

- **Uptime Kuma** checks service availability and protocol-level reachability.
- **Beszel** collects host-level resource metrics from both Debian virtual machines.

Homepage integrates the monitoring information into the central dashboard. The monitoring services run on the Debian services VM (VM 100), while the Beszel agent also runs on the AMP VM (VM 101).

Private LAN addresses, API credentials, authentication tokens, and Tailscale addresses are intentionally excluded from this public document.

## Monitoring Architecture

| Component | Host | Deployment | Purpose |
|---|---|---|---|
| Uptime Kuma | VM 100, `debian-services` | Docker Compose | Service availability checks and status page |
| Beszel Hub | VM 100, `debian-services` | Docker Compose | Metrics collection and dashboard |
| Beszel Agent | VM 100, `debian-services` | Docker container with host networking | Monitors the Debian services VM |
| Beszel Agent | VM 101, `amp-server` | Linux binary managed by systemd | Monitors the AMP VM |
| Homepage | VM 100, `debian-services` | Docker Compose | Aggregated dashboard and monitoring widgets |

### Uptime Kuma

| Property | Configuration |
|---|---|
| Container | `uptime-kuma` |
| Image | `louislam/uptime-kuma:2` |
| Deployment directory | `/opt/uptime-kuma` |
| Web interface | TCP 3001 |
| Persistent data | `/opt/uptime-kuma/data` mounted at `/app/data` |
| Restart policy | `unless-stopped` |
| Status page slug | `homelab` |

The status page is available through the private network at:

```text
http://<DEBIAN-SERVICES-LAN-IP>:3001/status/homelab
```

### Configured Uptime Kuma Monitors

Five monitors were configured and verified as Up.

| Monitor | Check Type | Target |
|---|---|---|
| Proxmox VE | HTTP(S) | Proxmox web interface |
| Homepage | HTTP | Homepage web interface |
| Technitium DNS | HTTP | Technitium web administration interface |
| AMP | HTTP | AMP web interface |
| Minecraft Server | GameDig | Minecraft Java Edition server |

**Monitoring limitations and exceptions**

- The Technitium monitor checks the HTTP administration interface. It does not independently test DNS resolution over TCP or UDP.
- The Proxmox monitor ignores the TLS certificate error because the local Proxmox web interface uses a self-signed certificate.
- The Minecraft monitor uses GameDig with the Minecraft Java Edition protocol.

### Beszel Hub

| Property | Configuration |
|---|---|
| Container | `beszel` |
| Image | `henrygd/beszel:latest` |
| Deployment directory | `/opt/beszel` |
| Web interface | TCP 8090 |
| Persistent data | `/opt/beszel/data` mounted at `/beszel_data` |
| Restart policy | `unless-stopped` |

The Hub manages two monitored systems: `debian-services` and `amp-server`.

### Beszel Agent — VM 100

The agent for `debian-services` runs as a Docker container.

| Property | Configuration |
|---|---|
| Container | `beszel-agent` |
| Image | `henrygd/beszel-agent` |
| Deployment directory | `/opt/beszel-agent` |
| Network mode | Host |
| Docker socket | Read-only mount for container discovery and metrics |
| Agent listen port | TCP 45876 |

The agent uses the generated Hub key and token. These values are stored in the local deployment configuration and are not included in this repository.

### Beszel Agent — VM 101

The AMP VM does not use Docker for its application environment, so its Beszel agent was installed as a standalone Linux binary.

| Property | Configuration |
|---|---|
| Binary | `/opt/beszel-agent/beszel-agent` |
| systemd unit | `beszel-agent.service` |
| Unit file | `/etc/systemd/system/beszel-agent.service` |
| Data directory | `/var/lib/beszel-agent` |
| Agent listen port | TCP 45876 |
| Startup behavior | Enabled at boot |

The service is managed with systemd:

```bash
sudo systemctl status beszel-agent
sudo systemctl restart beszel-agent
sudo journalctl -u beszel-agent -n 20 --no-pager
```

The generated authentication values are stored in the agent's local service configuration and are not documented here.

## Metrics Collected by Beszel

Beszel reports the following metrics for both Debian VMs:

- CPU utilization
- Memory utilization
- Disk utilization
- Average system load
- Network activity
- System uptime

The two agents allow the resource usage of the Docker services VM and the AMP/Minecraft VM to be viewed independently.

## Homepage Integration

Homepage displays three Beszel widgets:

1. **Beszel Overview** — total monitored systems and online count.
2. **debian-services** — CPU, memory, disk, and network metrics for VM 100.
3. **amp-server** — CPU, memory, disk, and network metrics for VM 101.

Homepage also displays the Uptime Kuma status widget and the existing Proxmox widget.

The Beszel widget authenticates using a Beszel superuser account. The credentials are stored in the local Homepage environment file at `/opt/homepage/.env` and referenced through environment-variable substitutions in the Homepage service configuration.

The environment file is restricted to the root account with file mode `600`. Credential values must never be committed to Git or copied into public documentation.

Homepage's Beszel environment variable names:

```text
HOMEPAGE_VAR_BESZEL_USERNAME
HOMEPAGE_VAR_BESZEL_PASSWORD
```

After changing the environment file, recreate the Homepage container so Docker passes the updated environment into the container:

```bash
cd /opt/homepage
sudo docker compose up -d --force-recreate homepage
```

A normal container restart does not reload environment variables that were changed in the host-side `.env` file.

## Verification and Testing

### Initial Deployment

- Confirmed the Beszel Hub web interface was accessible.
- Confirmed the VM 100 Docker agent started and established a WebSocket connection to the Hub.
- Confirmed the VM 101 systemd agent started and established a WebSocket connection to the Hub.
- Confirmed both systems appeared online and reported CPU, memory, disk, average load, network, and uptime metrics.
- Confirmed the Homepage overview showed two monitored systems with both online.
- Confirmed Homepage displayed per-system metrics for both VMs.
- Confirmed all five Uptime Kuma monitors reported Up during verification.

### VM 101 Reboot Persistence Test

The AMP VM was rebooted to verify systemd startup behavior.

The following checks passed:

- `beszel-agent.service` was enabled and active after reboot.
- The agent process started automatically.
- Disk and network interfaces were detected.
- The first WebSocket connection attempt occurred before network connectivity was ready and logged a temporary network-unreachable warning.
- The agent retried and successfully connected to the Beszel Hub after network connectivity became available.

This verified automatic startup and successful reconnection for the VM 101 agent. The initial connection warning was transient and resolved without manual intervention.

## Security and Operational Notes

- Monitoring interfaces are intended for private LAN or authorized remote access.
- Do not expose the Beszel Hub, Uptime Kuma, or Homepage administration interfaces directly to the public internet.
- Keep Beszel keys, tokens, superuser credentials, and Homepage environment files out of version control.
- The Docker socket mount gives the VM 100 Beszel agent visibility into Docker metadata; it is mounted read-only.
- The Proxmox host is not currently monitored by a Beszel agent. Beszel currently monitors the two Debian VMs only.
- Resource readings are time-dependent. Use historical graphs and trends rather than treating a single CPU or memory snapshot as a sustained condition.

## Troubleshooting Reference

| Symptom | Checks |
|---|---|
| Beszel system offline | Check agent service/container status, network connectivity, Hub reachability, and agent logs |
| VM 101 agent not running after reboot | Run `sudo systemctl status beszel-agent` and inspect the system journal |
| Homepage reports a Beszel API error | Verify the environment variables are present inside the Homepage container and recreate the container after changing `.env` |
| Homepage widgets load but show no system data | Verify the system is online in Beszel and that the configured system identifier matches the Hub |

Useful VM 100 Docker checks:

```bash
sudo docker ps --filter name=beszel-agent
sudo docker logs --tail=30 beszel-agent
```

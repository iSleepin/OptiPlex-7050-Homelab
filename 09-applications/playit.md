
# Playit.gg — Public Minecraft Access

## Overview

Playit.gg provides public connectivity to the Minecraft server hosted on the AMP VM without requiring port forwarding on the home router.

The Playit agent runs on the AMP server VM and establishes an outbound connection to the Playit network. Friends can connect to the Minecraft server using the public hostname assigned by Playit.

## Deployment Details

| Setting | Configuration |
|---|---|
| Host VM | `amp-server` |
| VM ID | `101` |
| Minecraft instance | `Linggango01` |
| Server software | AMP-managed Forge |
| Minecraft version | `1.20.1` |
| Forge version | `47.4.20` |
| Local destination | `127.0.0.1:25565` |
| Tunnel type | Minecraft Java |
| Playit agent | `amp-server-minecraft` |
| Public hostname | `your-server.example.com` |

> The public hostname above is a documentation placeholder, not the actual server address.

## Installation and Configuration

The Playit agent was installed on the Debian-based `amp-server` VM.

The agent was registered with the Playit account and connected successfully.

A Minecraft Java tunnel was created with the following destination:

- Local address: `127.0.0.1`
- Local port: `25565`
- Protocol: TCP
- Tunnel name: `Linggango-Minecraft-Java`

Playit automatically allocated the public connection address.

## Network Architecture

1. A player connects to the public Playit hostname.
2. Playit forwards the connection through its established tunnel.
3. The Playit agent on `amp-server` receives the connection.
4. Traffic is forwarded to the local Minecraft server at `127.0.0.1:25565`.
5. AMP manages the Minecraft server process.

## Verification

The following functionality was tested successfully:

- Playit agent connected to the Playit account.
- Minecraft Java tunnel created.
- Minecraft server running under AMP.
- Public connection successfully established.
- Connection tested from outside the home network.
- No router port forwarding required.

## Security Considerations

- Playit is used only for public Minecraft connectivity.
- AMP management access remains private through the LAN and Tailscale.
- Proxmox management access remains private.
- Technitium DNS administration remains private through Tailscale Serve.
- No router port forwarding is configured for these services.
- Agent credentials, authentication tokens, and private connection details must never be committed to Git.

## Maintenance

To check the agent service:

```bash
sudo systemctl status playit --no-pager
```

To inspect recent service logs:

```bash
sudo journalctl -u playit -n 50 --no-pager
```

Manage tunnels and review agent connectivity through the Playit dashboard.

## Notes

The public hostname in this document is intentionally fictional. The actual hostname and any associated private credentials are excluded from the public repository.

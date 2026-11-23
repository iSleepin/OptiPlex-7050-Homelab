
# Linggango Modded Minecraft Server

## Overview

A modded Minecraft Java Edition server is hosted through CubeCoders AMP on the homelab's dedicated game-server virtual machine.

The server uses the Linggango modpack and is managed through AMP rather than manually launching the Forge startup script.

Public Minecraft connectivity is provided through Playit.gg. Private server administration remains accessible through the local network and Tailscale.

## Deployment Environment

| Property | Configuration |
|---|---|
| Proxmox VM ID | 101 |
| VM Hostname | `amp-server` |
| Operating System | Debian 13 |
| vCPU | 4 |
| VM Memory | 12 GB |
| Virtual Disk | 100 GB |
| Server Management | CubeCoders AMP |
| LAN Server Address | `192.168.68.104` |

## Minecraft Configuration

| Property | Configuration |
|---|---|
| Edition | Minecraft Java Edition |
| Modpack | Linggango |
| Minecraft Version | 1.20.1 |
| Mod Loader | Minecraft Forge |
| Forge Version | 47.4.20 |
| Java Runtime | Eclipse Temurin 17 |
| AMP Instance | Minecraft |

The Forge version was verified against the extracted server pack's `run.sh` and Forge library directory.

The pack's README lists Forge 47.4.10, but the actual launcher script and extracted library directory specify 47.4.20. The extracted server-pack files were used as the source of truth.

## Server Pack Installation

The Linggango server-pack ZIP was uploaded through AMP's File Manager and extracted into the Minecraft instance directory.

The extracted pack includes:

- `config`
- `defaultconfigs`
- `kubejs`
- `libraries`
- `mods`
- `eula.txt`
- `run.bat`
- `run.sh`
- `server-icon.png`
- `user_jvm_args.txt`

The Minecraft EULA file was verified to contain:

```text
eula=true
```

The Linux startup script references the Forge argument file:

```bash
java @user_jvm_args.txt @libraries/net/minecraftforge/forge/1.20.1-47.4.20/unix_args.txt "$@"
```

AMP manages server startup. The `run.sh` and `run.bat` files are retained as part of the server pack and are not manually executed.

## Memory Configuration

The server pack recommends between 6 GB and 11 GB of RAM, with additional memory recommended as player count increases.

The initial AMP configuration uses:

| Setting | Configuration |
|---|---:|
| AMP Memory Limit | 8192 MB |
| AMP Memory Pre-allocation | 6144 MB |
| VM Memory | 12 GB |
| VM vCPU | 4 |

The server's memory allocation is intentionally limited to leave resources available for Debian and AMP.

The server pack's `user_jvm_args.txt` originally included:

```text
-Xmx8G
-Djava.net.preferIPv4Stack=true
```

AMP manages the Java heap allocation through its instance settings. Avoid adding duplicate heap-limit arguments.

## Initial Server Settings

Recommended starting values:

| Setting | Value |
|---|---:|
| View Distance | 8 chunks |
| Simulation Distance | 6 chunks |
| Maximum Players | 5 |

View distance and simulation distance should be adjusted according to actual CPU utilization, memory consumption, and player activity.

These are initial recommendations, not verified performance benchmarks.

## Server Access

### Local Network Access

The server is hosted at:

```text
192.168.68.104:25565
```

AMP management interface:

```text
http://192.168.68.104:8080
```

The AMP management interface is intended for private administration through the local network or Tailscale.

### Public Remote Access — Playit.gg

Playit.gg provides public connectivity to the Minecraft server without requiring inbound port forwarding on the home router.

The Playit agent runs on the `amp-server` VM and forwards incoming connections to the Minecraft server through the local loopback interface.

| Setting | Configuration |
|---|---|
| Tunnel Name | `Linggango-Minecraft-Java` |
| Agent Name | `amp-server-minecraft` |
| Tunnel Type | Minecraft Java |
| Local Address | `127.0.0.1` |
| Local Port | `25565` |
| Public Hostname | `your-server.example.com` |

The public hostname is intentionally fictional and does not represent the actual assigned Playit address.

Playit automatically allocates the public connection address.

Players connect using the assigned Playit hostname. The actual hostname is maintained outside this repository.

### Network Architecture

1. A remote player connects using the public Playit hostname.
2. Playit forwards the connection through its established tunnel.
3. The Playit agent on `amp-server` receives the forwarded connection.
4. Traffic is directed to `127.0.0.1:25565`.
5. AMP manages the Minecraft server process.

The Playit tunnel is dedicated to Minecraft connectivity. Administrative services are not exposed through this tunnel.

## Server Verification

The server was successfully started through AMP.

The deployment confirms:

- AMP can launch the Minecraft instance.
- The Linggango server-pack files are present.
- The server uses the intended Minecraft and Forge versions.
- The server is running under the selected Java runtime.
- The Minecraft process listens on TCP port `25565`.
- The Playit agent successfully connected.
- The Minecraft Java tunnel was created.
- Public connectivity was successfully tested from outside the home network.
- No router port forwarding was required.

## Backup Configuration

AMP's local backup retention settings have been configured:

| Setting | Value |
|---|---|
| Backup Count Limit | 24 |
| Backup Space Limit | 20,280 MB |
| Individual Backup Limit | 10,140 MB |
| Replacement Policy | Delete Single Oldest |
| Compression | Optimal |
| Quiesce Delay | 5 seconds |
| Backup Storage Path | `./Backups/` |
| Backup Staging Path | Default |

The hourly backup schedule and successful backup creation have not yet been verified.

See `08-backups/backup-strategy.md` for additional details.

## Operational Considerations

- Start and stop the server through AMP.
- Monitor the AMP console for Forge and mod-loading errors.
- Monitor VM memory and CPU utilization.
- Avoid allocating the entire VM memory to Minecraft.
- Keep the server pack's configuration and mod files consistent with the client modpack.
- Back up the world before making major mod or configuration changes.
- Keep AMP management access private.
- Use Playit.gg exclusively for public Minecraft connectivity.
- Do not expose administrative services through public tunnels.
- Do not commit Playit agent credentials, authentication tokens, or actual public connection details to Git.

## Future Improvements

- Verify automatic hourly backups.
- Perform a backup restoration test.
- Monitor memory and CPU usage during multiplayer sessions.
- Tune view distance and simulation distance based on performance.
- Establish independent backup storage.
- Document future modpack updates and configuration changes.
- Monitor Playit agent availability and tunnel connectivity.

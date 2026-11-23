# Linggango Modded Minecraft Server

## Overview

A modded Minecraft Java Edition server is hosted through CubeCoders AMP on the homelab's dedicated game-server virtual machine.

The server uses the Linggango modpack and is managed through AMP rather than manually launching the Forge startup script.

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
| Server Address | `192.168.68.104` |

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

The server is hosted at:

```text
192.168.68.104
```

AMP management interface:

`http://192.168.68.104:8080`

The server is currently intended for local network access.

Clients must use a compatible Minecraft version and the matching Linggango modpack.

## Server Verification

The server was successfully started through AMP.

The initial deployment confirms:

- AMP can launch the Minecraft instance.
- The Linggango server-pack files are present.
- The server uses the intended Minecraft and Forge versions.
- The server is running under the selected Java runtime.

Client connectivity, multiplayer behavior, and sustained performance testing remain to be documented.

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
- Do not expose the server publicly through router port forwarding without reviewing security and network requirements.

## Future Improvements

- Verify client connectivity from a Windows Minecraft client.
- Verify automatic hourly backups.
- Perform a backup restoration test.
- Monitor memory and CPU usage during multiplayer sessions.
- Tune view distance and simulation distance based on performance.
- Establish independent backup storage.
- Document future modpack updates and configuration changes.
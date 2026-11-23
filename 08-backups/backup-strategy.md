# Backup Strategy

## Overview

This document tracks the current backup configuration and future backup improvements for the OptiPlex 7050 Homelab.

The current deployment uses AMP's built-in backup functionality for the Linggango Minecraft server.

The long-term objective is to maintain recoverable backups independently of the virtual machine and primary storage.

## Current Backup Implementation

AMP manages local Minecraft server backups on Proxmox VM 101 (`amp-server`).

### Backup Configuration

| Setting | Configuration |
|---|---|
| Backup Count Limit | 24 |
| Backup Space Limit | 20,280 MB |
| Individual Backup Limit | 10,140 MB |
| Replacement Policy | Delete Single Oldest |
| Compression Level | Optimal |
| Quiesce Delay | 5 seconds |
| Backup Storage Path | `./Backups/` |
| Backup Staging Path | Default |

### Replacement Policy

The selected replacement policy is **Delete Single Oldest**.

When a backup limit is reached, AMP attempts to remove the oldest backup to make room for a new backup.

The backup count and storage limits both apply. Reaching a storage limit can prevent a new backup from being created if sufficient space cannot be recovered.

### Backup Storage Location

The configured storage path is:

```text
./Backups/
```

This is a relative path within the AMP instance directory.

The backup files are stored on the same virtual disk as the Minecraft instance.

This provides convenient local recovery but does not protect against failure of the VM's virtual disk or the underlying physical storage.

## Backup Schedule Status

The desired backup frequency is once per hour.

The AMP retention settings have been configured, but the scheduled task has not yet been independently verified.

A successful backup has also not yet been confirmed.

The following checks remain outstanding:

- Verify an AMP scheduled task runs every hour.
- Confirm that a backup completes successfully.
- Confirm that the backup appears in AMP's backup history.
- Record the size of the first successful backup.
- Verify that a backup can be restored.

Retention settings alone do not create a backup schedule.

## Backup Limitations

The current implementation has several limitations:

- Backups are stored within the AMP VM.
- The VM and backups share the same virtual disk.
- There is no verified independent backup copy.
- Backup restoration has not been tested.
- The configured storage limit may restrict the number of retained backups if backup sizes grow.

## Planned Backup Improvements

### Independent Backup Storage

A future improvement is to store backups outside the AMP VM.

Potential options include:

- Proxmox Backup Server
- A separate NAS or storage server
- An external backup drive
- Another independent network storage target

### Proxmox VM Backups

The AMP VM should eventually be included in a Proxmox-level backup strategy.

This would provide a recovery option for the complete virtual machine, including its operating system and AMP installation.

### Restore Testing

A backup is not considered verified until restoration has been tested.

Future testing should include:

1. Identify a known-good Minecraft backup.
2. Stop the Minecraft instance.
3. Restore the backup to the appropriate instance or test environment.
4. Start the restored server.
5. Verify that the world and mod configuration load successfully.

## Security Considerations

- Do not store passwords or authentication tokens in backup documentation.
- Restrict access to backup files.
- Protect backup storage from accidental deletion.
- Keep independent copies separate from the primary VM storage.
- Verify backup integrity and restoration capability.

## Current Status

| Item | Status |
|---|---|
| AMP local backup configuration | Configured |
| Backup retention limit | Configured |
| Replacement policy | Configured |
| Hourly scheduled task | Not yet verified |
| Successful backup creation | Not yet verified |
| Restore testing | Not yet completed |
| Independent backup storage | Planned |
| Proxmox VM backup strategy | Planned |

## Future Improvements

- Verify the hourly backup schedule.
- Confirm successful backup creation.
- Perform a Minecraft backup restoration test.
- Configure independent backup storage.
- Implement Proxmox VM backups.
- Document backup retention and recovery procedures.
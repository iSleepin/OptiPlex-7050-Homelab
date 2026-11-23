# Storage Health and Configuration

## Primary Storage Device

| Property | Result |
|---|---|
| Drive Identifier | KingFast |
| Drive Type | SATA SSD |
| Advertised Capacity | 1 TB |
| Reported Capacity | 953.87 GB |
| Interface | SATA |
| Partition Style | GPT |
| Windows Health Status | Healthy |
| Operational Status | OK |

## Verification Method

The drive was inspected using Windows PowerShell.

Commands used:

```powershell
Get-PhysicalDisk |
Select-Object FriendlyName, MediaType, Size, HealthStatus

Get-Disk |
Select-Object Number, FriendlyName, BusType, PartitionStyle, Size
```

## Verification Results

- Windows successfully detected the SSD.
- The drive was identified as a SATA SSD.
- The drive uses GPT partitioning.
- Windows reported the drive health status as Healthy.
- The drive operational status was reported as OK.

## Current Limitations

The Windows-reported Healthy status is an initial check, not a complete diagnostic.

The following checks have not yet been completed:

- [ ] Detailed SMART attribute inspection
- [ ] SSD wear-level assessment
- [ ] Power-on hours inspection
- [ ] Total host writes inspection
- [ ] SSD model and firmware identification

## Planned Storage Use

The existing 1 TB SATA SSD will initially serve as the primary storage device for the Proxmox VE installation and virtual machines.

Additional storage may be added later for:

- VM and container storage
- Application data
- Media storage
- Photo libraries
- Backups

## Important Notes

The existing Windows installation is stored on this SSD.

Installing Proxmox VE directly onto this drive will erase its existing partitions and data. Important files must be backed up before installation.

A separate backup storage device is recommended for protecting virtual machines and application data.

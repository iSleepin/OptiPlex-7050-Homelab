
# Changelog

All notable changes to this homelab project are documented here.

## 2026-11-22 — Proxmox VE Installation

### Added
- Completed hardware inventory for the Dell OptiPlex 7050 SFF.
- Verified CPU virtualization support and BIOS configuration.
- Installed Proxmox VE directly on the 1 TB SATA SSD.
- Configured the Proxmox management interface.
- Documented the initial storage configuration.
- Verified connectivity to the local gateway and the internet.
- Added initial Proxmox installation and configuration documentation.

### Infrastructure
- Proxmox hostname: `pve`
- Management IP: `192.168.68.200/24`
- Gateway: `192.168.68.1`
- DNS server: `192.168.68.1`
- Filesystem: ext4
- Storage: local and local-lvm

### Verification
- Successfully accessed the Proxmox web interface.
- Confirmed the host can reach the local gateway.
- Confirmed DNS resolution and internet connectivity.

### Notes
- Future services and virtual machines are planned but have not yet been deployed.
- No passwords, credentials, or hardware MAC addresses are included in this repository.

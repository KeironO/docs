# Backing up stuff in Proxmox

## Overview

This document outlines the backup configuration for our Proxmox virtualisation environment. All backups are stored on the dedicated backup storage to ensure data segregation and protection from primary storage failures.

!!! important
    **ALL backups MUST be configured to use the designated backup storage only.**
    Never store backups on the same storage as the running VMs.

## Backup Storage Configuration

All backups are exclusively stored on the designated backup storage volume. This provides:

- Physical separation from production storage
- Protection against primary storage failures
- Dedicated I/O capacity for backup operations

## Backup Schedules

We maintain two distinct backup schedules based on service criticality:

### Standard Services Backup Schedule

This schedule applies to non-critical services:

| Setting | Value |
|---------|-------|
| Schedule | Daily at 21:00 (9 PM) |
| Compression | ZSTRD (balanced compression/performance) |
| Mode | Snapshot |
| Notifications | Email on failure only |
| Retention Policy | • Keep 2 daily backups<br>• Keep 1 weekly backup<br>• Keep 1 monthly backup |

### Critical Services Backup Schedule

This schedule applies to mission-critical services requiring higher backup frequency:

| Setting | Value |
|---------|-------|
| Schedule | Every 2 hours (*/2.00) |
| Compression | ZSTRD (balanced compression/performance) |
| Mode | Snapshot |
| Notifications | Email on failure only |
| Retention Policy | • Keep 12 hourly backups<br>• Keep 1 daily backup<br>• Keep 1 monthly backup<br>• Keep 1 yearly backup |

## Backup Configuration Steps

To configure these backup schedules in Proxmox:

1. Navigate to Datacenter > Backup in the Proxmox web interface
2. Click "Add" to create a new backup job
3. Select the appropriate VMs based on criticality
4. Configure the schedule, storage, and retention settings as specified above
5. Enable the "Send email to" option with "Only send on error" selected
6. Set compression to "ZSTRD" under the Options tab
7. Enable "Snapshot" mode
8. Click "Create" to save the backup job

## Backup Verification Procedure

To ensure backup integrity:

1. Monthly test restoration of a non-critical VM to verify backup integrity
2. Quarterly test restoration of a critical VM to validate recovery procedures
3. Automated backup verification with logs review

## Troubleshooting

### Common Backup Failures

If backups fail, check:

1. Backup storage space availability
2. Network connectivity to backup storage
3. Proxmox backup logs at `/var/log/proxmox-backup/`
4. VM snapshot creation permission issues

### Restoration Procedure

To restore from backup:

1. Navigate to the Proxmox web UI
2. Select 'Datacenter' > 'Storage' > [Backup Storage]
3. Locate the desired backup file
4. Select 'Restore' and follow the wizard instructions
5. Verify VM functionality after restoration

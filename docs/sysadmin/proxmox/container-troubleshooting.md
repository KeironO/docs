# Proxmox Container Troubleshooting Guide

## Common Container Start Issues

After three years of Proxmox, I've ran into a couple issues when using containers. This little guide provides a systematic approach to diagnosing and resolving container start problems, with special attention to High Availability (HA) managed containers.

## Basic Container Start Troubleshooting

### Check Container Status

Begin by verifying the current status of the container:

```bash
pct status <container_id>
```

If the container shows as "stopped", proceed with further troubleshooting.

### Examine Container Configuration

Review the container's configuration for potential issues:

```bash
pct config <container_id>
```

Look for potentially problematic settings such as:
- Network configuration issues
- Storage allocation errors
- Resource constraints
- Privileged/unprivileged settings

### Check System Logs

Examine system logs for errors related to the container:

```bash
# Check LXC-specific logs (if they exist)
tail -n 100 /var/log/pve/lxc/<container_id>.log

# Check system journal for container service events
journalctl -e -u pve-container@<container_id>

# Check general system logs
grep -i "<container_id>" /var/log/syslog
```

### Verify Storage Accessibility

Ensure the container's storage is accessible and healthy:

```bash
# For ZFS storage
zfs list | grep <container_id>
zpool status

# For LVM storage
lvs
vgs
```

### Check for Lock Files

Verify if there are any lock files preventing container operations:

```bash
# Check for LXC lock files
ls -la /var/lock/lxc/

# Remove a specific lock file if needed
rm -f /var/lock/lxc/<container_id>.lock

# Use the unlock command
pct unlock <container_id>
```

### Mount and Inspect Container Filesystem

Mount the container filesystem to check if it's accessible:

```bash
pct mount <container_id>
ls -la /var/lib/lxc/<container_id>/rootfs/
```

A successful mount indicates that the container's filesystem is intact, which is good news for data preservation.

## Troubleshooting High Availability (HA) Managed Containers

### Identify if Container is HA-Managed

If a container doesn't start and shows messages like "Requesting HA start for CT <container_id>", it's likely under HA management.

Check HA status with:

```bash
# List all HA resources
ha-status

# Check specific container status
ha-status service vm:<container_id>
```

Also examine HA related tasks:

```bash
# Look for hastart tasks
pvesh get /nodes/$(hostname)/tasks --limit 10 | grep <container_id>
```

### Check HA Logs

Examine the HA manager logs for errors:

```bash
tail -n 100 /var/log/pve/ha-manager.log
```

### Remove Container from HA Management

If HA is preventing the container from starting properly, remove it from HA management:

```bash
# Try with vm: prefix
ha-manager remove vm:<container_id>

# If that fails, try with ct: prefix
ha-manager remove ct:<container_id>
```

### Temporarily Disable HA Services

In extreme cases, temporarily stopping HA services can help troubleshoot:

```bash
# Stop HA cluster resource manager
systemctl stop pve-ha-crm

# Stop HA local resource manager
systemctl stop pve-ha-lrm

# Try starting the container directly
pct start <container_id>

# Re-enable HA services after successful start
systemctl start pve-ha-crm
systemctl start pve-ha-lrm
```

## Advanced Troubleshooting

### Force Container Stop

If a container is in a "stuck" state, force stopping it may help:

```bash
pct stop <container_id> --force
```

### Restart Container-Related Services

Restarting relevant Proxmox services can resolve certain issues:

```bash
# Restart container service
systemctl restart pve-container

# Restart LXC service (if applicable)
systemctl restart lxc
```

### Check for Ongoing Tasks

Identify any tasks that might be conflicting with container operations:

```bash
pvesh get /nodes/$(hostname)/tasks --limit 10
```

### Examine Process Activity

Check for processes related to the container:

```bash
ps aux | grep <container_id>
```

## Recovery Options

### Backup Container Data

Before attempting more invasive recovery measures, backup important data:

```bash
# If container is mounted
rsync -av /var/lib/lxc/<container_id>/rootfs/ /path/to/backup/

# Or use Proxmox backup features
vzdump <container_id> --mode snapshot
```

### Clone Container

Create a clone to test if the issue is with the container configuration:

```bash
pct clone <container_id> <new_id>
```

### Recreate Container Config

If configuration is corrupt but data is intact:

```bash
# Back up the original config
cp /etc/pve/lxc/<container_id>.conf /etc/pve/lxc/<container_id>.conf.bak

# Use the GUI to recreate with correct settings, pointing to the existing storage
```

## Special Considerations

### Nested Virtualisation

If container uses nested virtualisation, ensure the feature is properly enabled:

```bash
# Check if enabled in config
grep -i "nesting" /etc/pve/lxc/<container_id>.conf

# Enable if needed
pct set <container_id> -features nesting=1
```

### Container Upgrade Issues

After OS upgrades, init systems may change, causing start failures:

```bash
# Check container OS type
pct config <container_id> | grep ostype

# Update if needed
pct set <container_id> -ostype debian
```

### Network Issues

Networking problems can prevent containers from starting properly:

```bash
# Verify bridge interface exists
ip link show

# Check bridge in container config
pct config <container_id> | grep net
```

## Preventative Measures

To avoid future container start issues:

1. Regularly back up containers using `vzdump`
2. Document container configurations
3. Test HA failover scenarios in a controlled environment
4. Keep Proxmox updated to the latest stable version
5. Monitor system resources to prevent overcommitment
6. Use resource limits appropriate for your hardware

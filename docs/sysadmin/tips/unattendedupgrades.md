Keeping Stuff Up To Date
======================================================

##  Introduction

Unattended Upgrades is a package that automates the process of
installing security updates, ensuring your system stays up-to-date
without manual intervention. This guide will walk you through the steps
to set up and configure Unattended Upgrades on both Debian and Fedora
systems.

## Installation

### Debian

Ensure that your system is running a Debian-based distribution, such as
Debian itself or Ubuntu. Then, install the Unattended Upgrades package
using the following commands:

``` bash
sudo apt update
sudo apt install unattended-upgrades
```

### Fedora

For Fedora, use the following commands:

``` bash
sudo dnf install dnf-automatic
sudo systemctl enable --now dnf-automatic.timer
```

## Configuration

### Debian

The configuration for Unattended Upgrades on Debian is done through the
`/etc/apt/apt.conf.d/50unattended-upgrades` file. Open this file in a
text editor:

``` bash
sudo vim /etc/apt/apt.conf.d/50unattended-upgrades
```

### Fedora

For Fedora, the configuration is done in the `/etc/dnf/automatic.conf` file. Open this file in a text editor:

## Applying Changes

### Debian

Save the changes to the configuration file and apply them by running:

``` bash
sudo dpkg-reconfigure -plow unattended-upgrades
```

### Fedora

For Fedora, the changes are automatically applied when you save the
configuration file.

## Testing

To test if Unattended Upgrades is working correctly, you can simulate an
upgrade:

### Debian

``` bash
sudo unattended-upgrades --dry-run --debug
```

### Fedora

``` bash
sudo dnf-automatic upgrade --downloadonly
```

This will show what packages would be upgraded without actually
installing them.


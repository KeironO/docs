# Getting Started with ansible.keiron.xyz

This guide will walk you through setting up and using the Ansible automation framework the homelab based on the `ansible.keiron.xyz` repository.

## Repository Overview

This repository contains Ansible playbooks, configurations, and roles for managing a homelab infrastructure. It's designed to automate routine tasks, ensure consistency across systems, and simplify the management of multiple devices.

## Repository Structure

```
ansible.keiron.xyz/
├── ansible.cfg                       # Main Ansible configuration
├── inventory/                        # Directory for inventory files
│   ├── group_vars/                   # Group-specific variables
│   │   ├── lxc-debian-containers/    # Variables for Debian containers
│   │   └── vault.yml                 # Encrypted sensitive variables
│   └── hosts                         # Inventory file listing all hosts
├── playbooks/                        # Task-specific playbooks
│   ├── check-disk-space.yml          # Monitor disk usage
│   ├── test-gotify.yml               # Test Gotify notifications
│   └── update-debian-containers.yml  # Update Debian systems
├── README.md                         # Main documentation
└── site.yml                          # Main playbook
```

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/KeironO/ansible.keiron.xyz.git
cd ansible.keiron.xyz
```

### 2. Install Ansible

Make sure Ansible is installed on your control node (the machine you'll run Ansible from):

```bash
# On Debian/Ubuntu
sudo apt update
sudo apt install ansible

# On Fedora
sudo dnf install ansible
```

### 3. Configure Ansible Hosts

Edit the inventory file to include hosts, if you have cloned from the repo this will already be filled.

```bash
nano inventory/hosts
```

Example inventory structure:

```ini
[lxc-debian-containers]
proxy ansible_host=192.168.178.98

[fedora-workstations]
keiron-workstation ansible_host=192.168.178.26
```

### 4. Set Up the Ansible Service Account

Before you can run playbooks, you need to set up the ansible user on each target host. You can use the provided setup script:

```bash
# Download the script to the target host
curl -O https://raw.githubusercontent.com/KeironO/ansible.keiron.xyz/main/setup_ansible_user.sh

# Make it executable and run it with sudo
chmod +x setup_ansible_user.sh
sudo ./setup_ansible_user.sh
```

The script will:
1. Create the ansible user
2. Configure passwordless sudo access
3. Set up the .ssh directory for authentication

Follow the script's prompts to set a password for the ansible user.

### 5. Set Up SSH Keys

On your control node, create a dedicated SSH key for Ansible if you haven't already:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/ansible_id
```

Then copy the public key to each target host:

```bash
ssh-copy-id -i ~/.ssh/ansible_id ansible@192.168.x.x
```

### 6. Test Connectivity

Verify that Ansible can connect to your hosts:

```bash
ansible all -m ping
```

## Using the Repository

### Basic Commands

List all hosts in your inventory:

```bash
ansible all --list-hosts
```

Run a playbook:

```bash
ansible-playbook playbooks/update-debian-containers.yml
```

Run a playbook on specific hosts:

```bash
ansible-playbook playbooks/update-debian-containers.yml -l proxy
```

### Working with Vault

This repository uses Ansible Vault to encrypt sensitive information.

Create a new encrypted file:

```bash
ansible-vault create inventory/group_vars/new_encrypted_file.yml
```

Edit an existing encrypted file:

```bash
ansible-vault edit inventory/group_vars/vault.yml
```

Run a playbook that uses encrypted variables:

```bash
ansible-playbook playbooks/test-gotify.yml --ask-vault-pass
```

## Adding New Hosts

1. Add the host to your inventory file
2. Set up the ansible user on the new host using the setup script
3. Copy your SSH key to the new host
4. Test connectivity with `ansible new-host -m ping`

## Creating New Playbooks

1. Create a new YAML file in the playbooks directory
2. Define your tasks and variables
3. Run the playbook with `ansible-playbook playbooks/your-new-playbook.yml`

## Troubleshooting

### Connection Issues

If you're having trouble connecting to hosts:

```bash
ansible all -m ping -vvv
```

### Sudo Issues

If you're having problems with sudo permissions:

```bash
ansible all -m command -a "sudo -l" -u ansible
```

### Playbook Execution Issues

For more verbose output during playbook execution:

```bash
ansible-playbook playbooks/your-playbook.yml -vvv
```

## Further Resources

- [Ansible Documentation](https://docs.ansible.com/)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
- [Ansible Galaxy](https://galaxy.ansible.com/) for community roles
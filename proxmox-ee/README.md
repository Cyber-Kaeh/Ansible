# Proxmox Execution Environment

An Ansible Execution Environment for managing Proxmox VE containers and VMs.

## Quick Start

### Prerequisites
- VS Code with Dev Containers extension
- Docker installed and running

### Setup

1. Open this folder in VS Code
2. When prompted, click "Reopen in Container" (or use Command Palette: `Dev Containers: Reopen in Container`)
3. Wait for the container to build and tools to install

### Build the EE

Once inside the Dev Container:

```bash
# Build the Execution Environment
ansible-builder build --tag proxmox-ee:latest --container-runtime docker -v 3

# Verify the build
docker images | grep proxmox-ee
```

### Test the EE

```bash
# Check installed collections
docker run --rm proxmox-ee:latest ansible-galaxy collection list

# Check Python packages
docker run --rm proxmox-ee:latest pip list | grep -i proxmox

# Run a simple test
docker run --rm proxmox-ee:latest ansible --version
```

## Project Structure

```
proxmox-ee/
├── .devcontainer/
│   └── devcontainer.json    # VS Code Dev Container config
├── execution-environment.yml # Main EE definition
├── requirements.yml          # Ansible collections
├── requirements.txt          # Python packages
├── bindep.txt               # System packages
└── README.md                # This file
```

## What's Included

### Ansible Collections
- `community.general` - Contains proxmox_* modules
- `ansible.posix` - POSIX utilities

### Python Packages
- `proxmoxer` - Proxmox API client
- `requests` - HTTP library

## Next Steps

After building the EE, you can:

1. Create an inventory file for your Proxmox hosts
2. Write playbooks using the `community.general.proxmox*` modules
3. Run playbooks with `ansible-navigator` using your custom EE

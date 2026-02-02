# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Ansible Execution Environment (EE) for managing Proxmox VE infrastructure. The project uses `ansible-builder` to create a containerized environment with all necessary dependencies for running Ansible playbooks against Proxmox hosts.

## Architecture

The EE is built using `ansible-builder` (version 3 format) with three dependency layers:

- **Ansible Collections** (`requirements.yml`): Collections providing Proxmox modules
- **Python Packages** (`requirements.txt`): Runtime dependencies including `proxmoxer` API client
- **System Packages** (`bindep.txt`): Platform-specific build dependencies

The base image is `quay.io/ansible/ansible-runner:latest`, which provides the Ansible execution framework.

## Development Environment

This project uses VS Code Dev Containers for development. The dev container (defined in `devcontainer.json`) provides:

- Python 3.14 base environment
- Docker-in-Docker support for building EE images
- Pre-installed tools: `ansible-builder`, `ansible-navigator`, `ansible-lint`
- VS Code extensions for Ansible and YAML development
- SSH key mounting from host for Git operations

## Common Commands

### Building the EE

```bash
# Build the execution environment image
ansible-builder build --tag proxmox-ee:latest --container-runtime docker -v 3

# Verify the build succeeded
docker images | grep proxmox-ee
```

### Testing the EE

```bash
# Verify Ansible collections are installed
docker run --rm proxmox-ee:latest ansible-galaxy collection list

# Verify Python packages are installed
docker run --rm proxmox-ee:latest pip list | grep -i proxmox

# Check Ansible version
docker run --rm proxmox-ee:latest ansible --version
```

### Using the EE

After building, use `ansible-navigator` to run playbooks with this EE:

```bash
ansible-navigator run playbook.yml --execution-environment-image proxmox-ee:latest
```

## Key Dependencies

- `community.general` collection (>=8.0.0): Provides `proxmox_*` modules for managing VMs, containers, and clusters
- `proxmoxer` (>=2.0.0): Python client library for the Proxmox API, required by the Ansible modules
- `ansible.posix` collection (>=1.5.0): Additional utilities for advanced operations

## File Modification Guidelines

- `execution-environment.yml`: Main EE definition. Version must remain `3`. Do not change base image unless necessary.
- `requirements.yml`: When adding collections, specify minimum versions using `>=` for forward compatibility.
- `requirements.txt`: Pin major versions for Python packages to avoid breaking changes.
- `bindep.txt`: System packages are platform-specific. Include both RHEL/CentOS and Debian/Ubuntu variants when needed.

# Ansible 

This directory contains files for creating an Ansible Execution Environment for running the k3s-ansible collection.

## Requirements

### System
* Podman or Docker installed
* python3
* python3-pip

### Pip
```
pip3 install ansible-builder
```
## Build 
(For a system running Docker)
```
ansible-builder build --container-runtime docker --tag k3s-install-ee:latest
```
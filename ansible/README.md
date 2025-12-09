# K3s Installation

This directory contains files for creating an Ansible Execution Environment for running the k3s-ansible collection.
This provides a reliable method for deploying the K3s cluster.

## Requirements
See: [Ansible Execution Environment Setup](https://docs.ansible.com/projects/ansible/latest/getting_started_ee/setup_environment.html)
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
cd execution-environment
ansible-builder build --container-runtime docker --tag k3s-install-ee:latest
```

## Run 
Once the execution envionment is built, it can be run by simply running the k3s install playbook inside the container:
```
docker run k3s-install-ee:latest ansible-playbook k3s.orchestration.site
```
Or from your local machine using ansible navigator:
```
ansible-navigator run --execution-environment-image k3s-install-ee:latest --pull-policy missing
```

## Help & Resources
* [GitHub: k3s-ansible](https://github.com/k3s-io/k3s-ansible)
* [Ansible Execution Environment Setup](https://docs.ansible.com/projects/ansible/latest/getting_started_ee/setup_environment.html)
* [Define Execution Environments](https://docs.ansible.com/projects/builder/en/stable/definition/)

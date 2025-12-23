# K3s Installation

This directory contains files for creating an Ansible Execution Environment for running the k3s-ansible collection.
This provides a reliable method for deploying the K3s cluster.

The `kubeconfig-export.yaml` playbook is used for the CI pipeline, to manually export the Kubeconfig file from the cluster, as k3s-ansible does not seem to export it properly when running in CI. 

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

## Configure
The `group_vars/k3s_cluster.yaml` file contains variables that are applied to the whole cluster, including the server and agent configurations defined in [K3s Configuration File](https://docs.k3s.io/installation/configuration#configuration-file)

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
* [K3s Docs](https://docs.k3s.io)
* [GitHub: k3s-ansible](https://github.com/k3s-io/k3s-ansible)
* [Ansible Execution Environment Setup](https://docs.ansible.com/projects/ansible/latest/getting_started_ee/setup_environment.html)
* [Define Execution Environments](https://docs.ansible.com/projects/builder/en/stable/definition/)

# k8s-bootstrap

A project for provisioning a Kubernetes cluster with core platform services, and preparing for hosting GitOps-based applications.

## Motivation

Previously, I had a project that used Terraform to install core services (such as CNIs) as well as apps onto Kubernetes clusters. 
The configuration files for provisioning the Kubernetes cluster also lived in that project. 

This project quickly became complex, with poor separation of responsibilities, as well as problems with large Terraform state being constantly applied to update applications.

I wanted to migrate the app management to a GitOps-based method, and simutaneously use this opportunity to create a more professional setup, with better security and automation.

## Aims
This project aims to create a configuration that:
* Provisions a Kubernetes cluster with Ansible
* Installs any core system services such as CNIs
* Installs Flux and bootstraps it, ready to deploy apps in a GitOps manner
* Carries out the above via a GitLab CI/CD pipeline 

## Prerequisites
* Remote machine/s suitable for running Kubernetes, accessible over SSH.
* A Git repository containing the Kubernetes manifests to be synced with the cluster (default path: clusters/homelab)

## Configuration
### K3s Install
[K3s Ansible](https://github.com/k3s-io/k3s-ansible) is used to install Kubernetes.

`ansible/inventory.yaml` should be configured to include the addresses of the machines you intend to be the server and agent nodes of your cluster.
`ansible/group_vars/k3s_cluster.yaml` should contain your desired k3s-ansible and K3s cluster configurations. 

Further details can be found in [K3s Install README](ansible/README.md).

### Flux Install
Flux is installed to the cluster via the [Flux Operator](https://fluxoperator.dev/docs/), and syncs to an existing Git repository containing Kubernetes mainfests.

`flux/flux-instance.yaml` should be configured to contain the details of the existing repository.

Further details can be found in [Flux Install README](flux/README.md).

### CI Pipeline Variables
| Variable | Description |
| --- | --- |
| ANSIBLE_USER | Ansible User to login to the remote Kubernetes host with. |
| SSH_KEY_B64 | Base 64-encoded SSH key that is permitted to SSH into the remote Kubernetes node. |
| EXEC_ENV_TAG | Version of the Ansible execution environment image to use. |
| HELM_KUBECTL_IMAGE | Name and tag of the image with Helm and Kubectl used to install Flux. |
| FLUX_OPERATOR_VERSION | Version of the Flux Operator to use. |
| GITLAB_TOKEN | GitLab Personal Access Token for the Kubernetes manifests repository with `Maintainer` role and `api, read_api, read_repository, write_repository` permissions. This allows Flux to authenticate to GitLab to sync the repository. |
| GITLAB_CERT_B64 | Base 64-encoded public certificate for the GitLab server, allowing Flux to verify the GitLab server certificate.|
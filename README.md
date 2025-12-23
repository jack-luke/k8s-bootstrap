# k8s-bootstrap

A GitLab CI pipeline that provisions a K3s Kubernetes cluster, and configured GitOps-based application deployment with Flux.

## Description
This project consists of a GitLab CI pipeline that:
* Provisions a K3s Kubernetes cluster with [K3s Ansible](https://github.com/k3s-io/k3s-ansible) inside an Ansible execution environment
* Installs [Flux Operator](https://fluxoperator.dev/docs/) to the cluster
* Configures Flux to sync with an existing Git repository containing manifests for the cluster applications and services.

### Motivation
Previously, I had managed my homelab cluster with a single repository that contained Ansible playbooks for installing K3s, and Terraform modules for installing Kubernetes services with the Kubernetes Terraform provider.

As the homelab grew, I found that the large Terraform state made updating applications slow. 
Furthermore, this system involved manually applying cluster & application state instead of using GitOps.

So, this project aims to solve these problems by migrating the installation of my homelab cluster and its applications to a GitLab CI pipeline.
Additionally, the application state is managed in a GitOps manner in its own repository, for better portability and separation of responsibilities.

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
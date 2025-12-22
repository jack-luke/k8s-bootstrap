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
* A remote machine suitable for running Kubernetes, accessible over SSH.
* A GitLab repository containing the Kubernetes manifests to be synced with the cluster (default path: clusters/homelab)

## Pipeline Configuration
| Variable | Description |
| --- | --- |
| ANSIBLE_USER | Ansible User to login to the remote Kubernetes host with. |
| SSH_KEY_B64 | Base 64-encoded SSH key that is permitted to SSH into the remote Kubernetes node. |
| EXEC_ENV_TAG | Version of the Ansible execution environment image to use. |
| HELM_KUBECTL_IMAGE | Name and tag of the image with Helm and Kubectl used to install Flux. |
| FLUX_OPERATOR_VERSION | Version of the Flux Operator to use. |
| GITLAB_TOKEN | GitLab Personal Access Token for the Kubernetes manifests repository with `Maintainer` role and `api, read_api, read_repository, write_repository` permissions. This allows Flux to authenticate to GitLab to sync the repository. |
| GITLAB_CERT_B64 | Base 64-encoded public certificate for the GitLab server, allowing Flux to verify the GitLab server certificate.|
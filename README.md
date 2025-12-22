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

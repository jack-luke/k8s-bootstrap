# Flux CD

## Installation 
### Flux Operator
The Flux Operator can be installed by following: [Flux Operator Install](https://fluxoperator.dev/docs/guides/install/).

This enables Flux to be managed using Custom Resource Definitions.

### Flux Instance
The `flux-instance.yaml` file contains the manifest for a FluxInstance, which installs Flux Controllers, and configures Flux to sync with an existing Git repository. 
This can be applied like so, and is applied to the `flux-system` namespace by default:
```bash
kubectl apply -f flux-instance.yaml
```

## Configuration
### Git Repository
The Flux Instance is configured to sync with a Git repository containing Kubernetes manifests to deploy to a cluster.
See: [Flux Operator Sync from a Git Repository](https://fluxoperator.dev/docs/instance/sync/#sync-from-a-git-repository)

| Parameter | Value | Example |
| --- | --- | --- |
| `.sync.url` | URL of the Git repository to sync with. | `https://gitlab.com/username/repository.git` |
| `.sync.path`| The path of a valid Kustomization YAML in the Git repository that points to the resources to deploy. | `clusters/prod` |
| `.sync.ref` | The Git ref which should be used to deploy the manifests from. | `refs/heads/main` |
| `.sync.pullSecret` | The name of a secret in the `flux-system` namespace that contains credentials to authenticate to the Git server.  | `git-auth` | 

## Help & Resources
* [Flux Docs](https://fluxcd.io/flux/)
* [Flux Operator Docs](https://fluxoperator.dev/docs/)
# ic-gitops-central
InfraCoders Central GitOps Repository

Below documentation is [Flux documentation](https://fluxcd.io/flux/) summary.

## Possible Flux repositories structures

Monorepo, repo per environment, repo per team, repo per app [Link](https://fluxcd.io/flux/guides/repository-structure/#repo-per-team).

## Flux Components (ToolKit Components)

Flux consists of multiple components which together to provide Continous Delivery on Kubernetes - [Flux - components](https://fluxcd.io/flux/components/).

### Source Controllers

Provides a common interface for artifacts acquisition (delivered as API endpoints), those can be:

- Git repositories
- OCI Repositories
- Buckets
- Helm Repositories
- Helm Charts
- External Artifacts - for 3rd party controllers
- Artifact Generators - can be used to compose (from multiple source) or splitting source into multiple deployable artifact

From Flux v2.7.0 can be extended with [Source Watcher](https://fluxcd.io/flux/components/source/#source-watcher).

Usually (if not awalys) authentication with credentials is supported, respirce refresh intervals can be adjusted, minimal refresh interval is 1 minute.

### Kustomize Controller

Manages infrastructure and workloads defined with Kubernetes manifests and assembled with Kustomize.

### Helm Controller

Watches for Helm Releases.
Supports HelmChart artifacts produced by HelmRepostiries and GitRepositories from Source controllers.

### Notification Controller

Handles inbound and outbound events.
Notifies toolkig controllers about source changes, can send events to external systems - Slack, MS Team, etc.

### Image reflector and automation controllers

Scans image reopsitories and reflects image metadata in Kubernetes sources.
Image Automation Controller updates yaml files based on the latest images scanned and commits the changes to a given Git repository.
E.g. one can define a policy, for an image repository, to define version range.

## Examples

### sample Helm release

```shell
# create HelmRepository resource to be later used by helm release
kubectl create -f ./examples/podinfo-helmrepository.yaml

# create HelmRelease
kubectl create -f ./examples/podinfo-helmrelease-v4.yaml

# upgrade HelmRelease (there's some error and helm is upgraded but ends up with error)
kubectl apply -f ./examples/podinfo-helmrelease-v5.yaml
```

### OCIRepository and Kustomization sample

It will also deploy some basic UI for Flux as Flux itself doesn't provide it on it's own (ArgoCD does):

```shell
# create OCIRegistry
kubectl create -f ./examples/capacitor-ocirepository.yaml

# create Kustomization
kubectl create -f ./examples/kustomization-ocirepository.yaml

```

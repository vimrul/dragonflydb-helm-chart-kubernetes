# DragonflyDB Helm Reference for Kubernetes

A minimal Helm chart for deploying a single DragonflyDB instance with a Kubernetes Deployment and NodePort Service.

**Scope:** a small deployment reference. It does not configure replication, persistent storage, authentication, or a high-availability topology.

## Repository layout

The chart lives in [dragonfly-helm/](dragonfly-helm/), not the repository root.

| File | Purpose |
| --- | --- |
| [Chart.yaml](dragonfly-helm/Chart.yaml) | Chart metadata |
| [values.yaml](dragonfly-helm/values.yaml) | Image repository, tag, and pull policy |
| [deployment.yaml](dragonfly-helm/templates/deployment.yaml) | Single replica and fixed resource settings |
| [service.yaml](dragonfly-helm/templates/service.yaml) | Service port 6379, target port 6379, NodePort 30082 |
| [_helpers.tpl](dragonfly-helm/templates/_helpers.tpl) | Resource naming, including optional nameOverride |

## Inspect and install

Prerequisites: an authorized Kubernetes context, Helm, and kubectl. Start in a disposable development cluster.

```bash
git clone https://github.com/vimrul/dragonflydb-helm-chart-kubernetes.git
cd dragonflydb-helm-chart-kubernetes
helm lint ./dragonfly-helm
helm template dragonflydb ./dragonfly-helm --namespace dragonflydb-namespace
helm install dragonflydb ./dragonfly-helm \
  --namespace dragonflydb-namespace --create-namespace
kubectl get deployments,pods,svc -n dragonflydb-namespace
```

With the default name, forward the service:

```bash
kubectl port-forward svc/dragonflydb 6379:6379 -n dragonflydb-namespace
```

Use a Redis-compatible client to check connectivity at localhost:6379.

## Configuration boundaries

Only image settings are defined in the supplied values file; the helper also accepts `nameOverride`. Replica count, resource settings, and service type are hard-coded in templates. Setting `replicaCount`, `resources`, or `service.type` in a values file does not change those templates.

The Deployment declares container port 6382 while the Service targets 6379. A container-port declaration does not configure the application's listening port: verify the process and align the manifests for your deployment.

Review the fixed NodePort and network reachability before installation. The chart pins image version v1.21.0; assess compatibility and maintenance status before choosing a version for real workloads.

## Verification and recovery

After installation, inspect pod events/logs, service endpoints, and client connectivity. Use test data to observe what happens when the pod restarts: this chart does not promise durable storage or automatic data recovery.

If deployment fails, inspect the rendered manifests and pod events before retrying. Restore a known configuration or Helm revision when applicable. Uninstalling the release removes the chart's resources:

```bash
helm uninstall dragonflydb --namespace dragonflydb-namespace
```

No cluster deployment, load test, or recovery benchmark is claimed by this README.

Author: [Mohammad Imrul Hasan](https://imrul.net/).


# DragonflyDB Helm Chart for Kubernetes

This repository contains a Helm chart to deploy [DragonflyDB](https://github.com/dragonflydb/dragonfly) on a Kubernetes cluster. DragonflyDB is a high-performance in-memory database compatible with the Redis protocol, designed for large-scale workloads.

## Features

- **Kubernetes Deployment**: Helm chart for deploying DragonflyDB on Kubernetes.
- **Scalable**: Easily scalable to meet performance requirements.
- **Customizable**: Configuration options provided via `values.yaml` for customizing the deployment.
- **Service Exposure**: Deploys a Kubernetes service to expose DragonflyDB.

## Prerequisites

- A Kubernetes cluster
- Helm installed and configured
- `kubectl` installed and configured

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/dragonflydb-helm-chart-kubernetes.git
cd dragonflydb-helm-chart-kubernetes
```

### 2. Install the Helm Chart

To install the DragonflyDB chart, run:

```bash
helm install dragonflydb ./ --namespace dragonflydb-namespace
```

This command installs DragonflyDB into your Kubernetes cluster within the specified namespace (`dragonflydb-namespace`). You can replace the namespace name with your preferred one.

### 3. Verify the Installation

After installation, check the status of the DragonflyDB pods:

```bash
kubectl get pods -n dragonflydb-namespace
```

### 4. Access DragonflyDB

Once the deployment is complete, you can expose DragonflyDB by using the service created by the Helm chart:

```bash
kubectl get svc -n dragonflydb-namespace
```

Forward the service port to access DragonflyDB:

```bash
kubectl port-forward svc/dragonflydb-service 6379:6379 -n dragonflydb-namespace
```

You can now connect to DragonflyDB using a Redis client at `localhost:6379`.

## Customization

The Helm chart comes with several configuration options that can be adjusted by modifying the `values.yaml` file. Some key configurable parameters are:

- **replicaCount**: Number of DragonflyDB instances to deploy.
- **resources**: Resource limits and requests for the DragonflyDB pods.
- **service.type**: The type of Kubernetes service (e.g., `ClusterIP`, `NodePort`, `LoadBalancer`).
  
You can override the default values by providing your custom `values.yaml` during installation:

```bash
helm install dragonflydb ./ -f custom-values.yaml --namespace dragonflydb-namespace
```

## Files Overview

- `Chart.yaml`: Contains metadata about the Helm chart.
- `values.yaml`: Default configuration values for the chart.
- `templates/deployment.yaml`: Kubernetes Deployment for DragonflyDB.
- `templates/service.yaml`: Kubernetes Service to expose DragonflyDB.
- `_helpers.tpl`: Helper template for consistent templating across the chart.

## Uninstalling the Chart

To uninstall/delete the `dragonflydb` release, run:

```bash
helm uninstall dragonflydb --namespace dragonflydb-namespace
```

This will remove all components associated with the DragonflyDB deployment.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributions

Contributions are welcome! Feel free to open an issue or submit a pull request.

## Author

[Imrul](https://imrul.net)

## Contact

For any inquiries, please contact me at [hello@imrul.net](mailto:hello@imrul.net).

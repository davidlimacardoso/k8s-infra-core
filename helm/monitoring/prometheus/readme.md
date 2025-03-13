# Prometheus Helm Chart

This Helm chart installs Prometheus, an open-source systems monitoring and alerting toolkit.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+

## Installation

To install the chart with the release name `my-prometheus`:

```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade -i prometheus  prometheus-community/prometheus -f prometheus/values.yaml  --namespace monitoring --create-namespace
```

## Configuration

The following table lists the configurable parameters of the Prometheus chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `alertmanager.enabled` | Enable Alertmanager | `true` |
| `server.persistentVolume.enabled` | Enable persistent storage | `true` |
| `server.persistentVolume.size` | Size of persistent storage | `8Gi` |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```sh
helm install my-prometheus \
    --set server.persistentVolume.size=10Gi \
    prometheus-community/prometheus
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```sh
helm install my-prometheus -f values.yaml prometheus-community/prometheus
```

## Persistent Volume Configuration

You can also configure persistent volumes using a YAML file. Below is an example `pv.yaml` file:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: prometheus-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/prometheus
```

To apply the persistent volume configuration, use the following command:

```sh
kubectl apply -f pv.yaml
```

## Uninstallation

To uninstall/delete the `my-prometheus` deployment:

```sh
helm uninstall my-prometheus
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## pv.yaml

Below is the content of the `pv.yaml` file:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: prometheus-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/prometheus
```

To apply the persistent volume configuration, use the following command:

```sh
kubectl apply -f pv.yaml
```

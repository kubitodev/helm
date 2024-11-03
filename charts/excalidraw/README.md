# Excalidraw

Virtual whiteboard for sketching hand-drawn like diagrams.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install excalidraw kubitodev/excalidraw
```

## Introduction

An open source virtual hand-drawn style whiteboard. Collaborative and end-to-end encrypted.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `excalidraw`:

```console
helm install excalidraw kubitodev/excalidraw
```

The command deploys excalidraw on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `excalidraw` deployment:

```console
helm delete excalidraw
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Excalidraw parameters

| Name                                            | Description                                                                                                                         | Value                    |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| `replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                      |
| `image.repository`                              | The Docker repository to pull the image from.                                                                                       | `excalidraw/excalidraw`  |
| `image.tag`                                     | The image tag to use.                                                                                                               | `latest`                 |
| `image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`           |
| `imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                     |
| `deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`               |
| `serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                   |
| `serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                     |
| `serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                     |
| `podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                     |
| `podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                     |
| `securityContext`                               | The security context to use for the container.                                                                                      | `{}`                     |
| `initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                     |
| `service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`              |
| `service.port`                                  | The port on which the service will run.                                                                                             | `8080`                   |
| `service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                     |
| `ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                  |
| `ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                     |
| `ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                     |
| `ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`    |
| `ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                      |
| `ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific` |
| `ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                     |
| `resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                     |
| `autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                  |
| `autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                      |
| `autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                    |
| `autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                     |
| `autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                     |
| `nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                     |
| `tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                     |
| `affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                     |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm install example \
  --set user=example \
  --set password=example \
    kubitodev/example
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
helm install example -f values.yaml kubitodev/example
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## License

Copyright &copy; 2024 Kubito

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

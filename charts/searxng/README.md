# SearXNG

Privacy-respecting, hackable metasearch engine. A metasearch engine (or search aggregator) is an online information retrieval tool that uses the data of a web search engine to produce its own results. Metasearch engines take input from a user and immediately query search engines for results. Sufficient data is gathered, ranked, and presented to the users.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install searxng kubitodev/searxng
```

## Introduction

This chart helps you create a metasearch engine that will take input from a user and immediately query search engines for results. Sufficient data is gathered, ranked, and presented to the users. Very useful in today's world for self hosted instances of LLM WebUI applications, such as Open Web UI.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `searxng`:

```console
helm install searxng kubitodev/searxng
```

The command deploys searxng on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `searxng` deployment:

```console
helm delete searxng
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Image parameters

| Name                    | Description                                   | Value             |
| ----------------------- | --------------------------------------------- | ----------------- |
| `image.repository`      | The Docker repository to pull the image from. | `searxng/searxng` |
| `image.tag`             | The image tag to use.                         | `latest`          |
| `image.imagePullPolicy` | The logic of image pulling.                   | `IfNotPresent`    |

### Common parameters

| Name                         | Description                                                                                                             | Value  |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------ |
| `serviceAccount.enabled`     | Specifies whether a ServiceAccount should be created.                                                                   | `true` |
| `serviceAccount.automount`   | Whether to automount the service account.                                                                               | `true` |
| `serviceAccount.annotations` | Annotations for service account. Evaluated as a template. Only used if `create` is `true`.                              | `{}`   |
| `serviceAccount.name`        | The name of the ServiceAccount to use. If not set and enabled is true, a name is generated using the fullname template. | `""`   |

### Deployment parameters

| Name                                            | Description                                                                                    | Value       |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------- |
| `replicaCount`                                  | The number of replicas to deploy.                                                              | `1`         |
| `imagePullSecrets`                              | Private registry pull secrets to include.                                                      | `[]`        |
| `env`                                           | Additional environment variables to add to the deployment.                                     | `[]`        |
| `podAnnotations`                                | Additional pod annotations.                                                                    | `{}`        |
| `podLabels`                                     | Additional pod labels.                                                                         | `{}`        |
| `podSecurityContext`                            | Optional pod security context.                                                                 | `{}`        |
| `securityContext`                               | Optional deployment security context.                                                          | `{}`        |
| `service.type`                                  | The type of the service to use.                                                                | `ClusterIP` |
| `service.port`                                  | The service port to use.                                                                       | `8080`      |
| `config.settings.enabled`                       | Enable custom settings.                                                                        | `true`      |
| `config.settings.data`                          | Custom settings data. Replace the secret key to any random string with `openssl rand -hex 32`. | `{}`        |
| `config.limiter.enabled`                        | Enable IP limiter.                                                                             | `true`      |
| `config.limiter.data`                           | IP limiter data.                                                                               | `{}`        |
| `config.uwsgi.enabled`                          | Enable UWSGI.                                                                                  | `true`      |
| `config.uwsgi.data`                             | UWSGI data.                                                                                    | `{}`        |
| `ingress.enabled`                               | Enable ingress record generation for Searxng.                                                  | `false`     |
| `ingress.className`                             | The class name of the ingress to use.                                                          | `""`        |
| `ingress.annotations`                           | Mapped annotations for the ingress.                                                            | `{}`        |
| `ingress.hosts`                                 | Array style hosts for the ingress.                                                             | `nil`       |
| `ingress.tls`                                   | Array style TLS secrets for the ingress.                                                       | `[]`        |
| `resources`                                     | Optional resource requests and limits to set.                                                  | `{}`        |
| `livenessProbe.httpGet.path`                    | The path to use for the liveness probe.                                                        | `/`         |
| `livenessProbe.httpGet.port`                    | The port name to use for the liveness probe.                                                   | `http`      |
| `readinessProbe.httpGet.path`                   | The path to use for the readiness probe.                                                       | `/`         |
| `readinessProbe.httpGet.port`                   | The port name to use for the readiness probe.                                                  | `http`      |
| `autoscaling.enabled`                           | Whether to enable autoscaling.                                                                 | `false`     |
| `autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                    | `1`         |
| `autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                    | `100`       |
| `autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                  | `80`        |
| `autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                               | `80`        |
| `volumes`                                       | Additional volumes to use.                                                                     | `[]`        |
| `volumeMounts`                                  | Additional volume mounts to use.                                                               | `[]`        |
| `nodeSelector`                                  | Optional node selector to use.                                                                 | `{}`        |
| `tolerations`                                   | Whether to set node tolerations.                                                               | `[]`        |
| `affinity`                                      | Whether to set node affinity.                                                                  | `{}`        |

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

## Configuration and installation details

Coming soon.

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

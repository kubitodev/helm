# Servarr

A Helm chart for deploying the Servarr suite of applications - including Sonarr, Radarr, Lidarr, Prowlarr, Readarr, and Jellyfin. These applications provide media management, automation, and streaming capabilities for TV shows, movies, music, books, and more. The chart also supports enabling additional services like Bazarr for subtitle management, FlareSolverr for handling anti-bot protections, Jellyseerr for media requests, qBittorrent for downloading, and other complementary applications to create a complete media server stack.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install servarr kubitodev/servarr
```

## Introduction

This chart helps you create a media server stack for your home media library, including TV shows, movies, music, books, and more.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `servarr`:

```console
helm install servarr kubitodev/servarr
```

The command deploys servarr on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `servarr` deployment:

```console
helm delete servarr
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Jellyfin parameters

| Name                                            | Description                                                                                                                                                                                                        | Value                          |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------ |
| `jellyfin.enabled`                              | Whether to enable Jellyfin.                                                                                                                                                                                        | `true`                         |
| `jellyfin.replicaCount`                         | The number of replicas to deploy.                                                                                                                                                                                  | `1`                            |
| `jellyfin.image.repository`                     | The Docker repository to pull the image from.                                                                                                                                                                      | `lscr.io/linuxserver/jellyfin` |
| `jellyfin.image.tag`                            | The image tag to use.                                                                                                                                                                                              | `10.10.1`                      |
| `jellyfin.image.pullPolicy`                     | The logic of image pulling.                                                                                                                                                                                        | `IfNotPresent`                 |
| `jellyfin.enableDLNA`                           | Whether to enable DLNA which requires the pod to be attached to the host network in order to be useful - this can break things like ingress to the service https://jellyfin.org/docs/general/networking/dlna.html. | `false`                        |
| `jellyfin.service.type`                         | The type of service to create.                                                                                                                                                                                     | `ClusterIP`                    |
| `jellyfin.service.port`                         | The port on which the service will run.                                                                                                                                                                            | `8096`                         |
| `jellyfin.service.nodePort`                     | The nodePort to use for the service. Only used if service.type is NodePort.                                                                                                                                        | `""`                           |
| `jellyfin.service.annotations`                  | Additional annotations to add to the service.                                                                                                                                                                      | `{}`                           |
| `jellyfin.service.labels`                       | Additional labels to add to the service.                                                                                                                                                                           | `{}`                           |
| `jellyfin.service.loadBalancerIP`               | The IP address to use for the LoadBalancer service. Only used if service.type is LoadBalancer.                                                                                                                     | `""`                           |
| `jellyfin.service.loadBalancerSourceRanges`     | The source ranges to use for the LoadBalancer service. Only used if service.type is LoadBalancer.                                                                                                                  | `[]`                           |
| `jellyfin.service.externalTrafficPolicy`        | The external traffic policy to use for the service. Set to either Cluster or Local.                                                                                                                                | `nil`                          |
| `jellyfin.ingress.enabled`                      | Whether to create an ingress for the service.                                                                                                                                                                      | `false`                        |
| `jellyfin.ingress.labels`                       | Additional labels to add to the ingress.                                                                                                                                                                           | `{}`                           |
| `jellyfin.ingress.annotations`                  | Additional annotations to add to the ingress.                                                                                                                                                                      | `{}`                           |
| `jellyfin.ingress.path`                         | The path to use for the ingress.                                                                                                                                                                                   | `/`                            |
| `jellyfin.ingress.hosts`                        | The hosts to use for the ingress.                                                                                                                                                                                  | `["chart-example.local"]`      |
| `jellyfin.ingress.tls`                          | The TLS configuration for the ingress.                                                                                                                                                                             | `[]`                           |
| `jellyfin.persistence.config.enabled`           | Whether to enable persistence for the config.                                                                                                                                                                      | `true`                         |
| `jellyfin.persistence.config.storageClass`      | The storage class to use for the config.                                                                                                                                                                           | `""`                           |
| `jellyfin.persistence.config.existingClaim`     | The name of an existing claim to use for the config.                                                                                                                                                               | `""`                           |
| `jellyfin.persistence.config.subPath`           | The subPath to use for the config's volume mount.                                                                                                                                                                  | `""`                           |
| `jellyfin.persistence.config.accessMode`        | The access mode to use for the config.                                                                                                                                                                             | `ReadWriteOnce`                |
| `jellyfin.persistence.config.size`              | The size to use for the config.                                                                                                                                                                                    | `1Gi`                          |
| `jellyfin.persistence.config.labels`            | Additional labels to add to the config.                                                                                                                                                                            | `{}`                           |
| `jellyfin.persistence.config.annotations`       | Additional annotations to add to the config.                                                                                                                                                                       | `{}`                           |
| `jellyfin.persistence.media.enabled`            | Whether to enable persistence for the media.                                                                                                                                                                       | `true`                         |
| `jellyfin.persistence.media.storageClass`       | The storage class to use for the media.                                                                                                                                                                            | `""`                           |
| `jellyfin.persistence.media.existingClaim`      | The name of an existing claim to use for the media.                                                                                                                                                                | `""`                           |
| `jellyfin.persistence.media.subPath`            | The subPath to use for the media's volume mount.                                                                                                                                                                   | `""`                           |
| `jellyfin.persistence.media.accessMode`         | The access mode to use for the media. Shared volume between all servarr applications, if running on different nodes, set to ReadWriteMany.                                                                         | `ReadWriteOnce`                |
| `jellyfin.persistence.media.size`               | The size to use for the media.                                                                                                                                                                                     | `10Gi`                         |
| `jellyfin.persistence.media.labels`             | Additional labels to add to the media.                                                                                                                                                                             | `{}`                           |
| `jellyfin.persistence.media.annotations`        | Additional annotations to add to the media.                                                                                                                                                                        | `{}`                           |
| `jellyfin.persistence.extraExistingClaimMounts` | Additional existing claim mounts to add to the pod.                                                                                                                                                                | `[]`                           |
| `jellyfin.resources`                            | The resources to use for the pod.                                                                                                                                                                                  | `{}`                           |
| `jellyfin.nodeSelector`                         | The node selector to use for the pod.                                                                                                                                                                              | `{}`                           |
| `jellyfin.tolerations`                          | The tolerations to use for the pod.                                                                                                                                                                                | `[]`                           |
| `jellyfin.affinity`                             | The affinity to use for the pod.                                                                                                                                                                                   | `{}`                           |
| `jellyfin.extraVolumes`                         | Additional volumes to add to the pod.                                                                                                                                                                              | `[]`                           |
| `jellyfin.extraVolumeMounts`                    | Additional volume mounts to add to the pod.                                                                                                                                                                        | `[]`                           |
| `jellyfin.extraEnvVars`                         | Additional environment variables to add to the pod.                                                                                                                                                                | `[]`                           |
| `jellyfin.extraInitContainers`                  | Additional init containers to add to the pod.                                                                                                                                                                      | `{}`                           |
| `jellyfin.extraContainers`                      | Additional sidecar containers to add to the pod.                                                                                                                                                                   | `{}`                           |
| `jellyfin.podSecurityContext`                   | The security context to use for the pod.                                                                                                                                                                           | `{}`                           |
| `jellyfin.securityContext`                      | The security context to use for the container.                                                                                                                                                                     | `{}`                           |
| `jellyfin.livenessProbe.enabled`                | Whether to enable the liveness probe.                                                                                                                                                                              | `false`                        |
| `jellyfin.livenessProbe.failureThreshold`       | The number of times to retry before giving up.                                                                                                                                                                     | `3`                            |
| `jellyfin.livenessProbe.initialDelaySeconds`    | The number of seconds to wait before starting the probe.                                                                                                                                                           | `10`                           |
| `jellyfin.livenessProbe.periodSeconds`          | The number of seconds between probe attempts.                                                                                                                                                                      | `10`                           |
| `jellyfin.livenessProbe.successThreshold`       | The minimum consecutive successes required to consider the probe successful.                                                                                                                                       | `1`                            |
| `jellyfin.livenessProbe.timeoutSeconds`         | The number of seconds after which the probe times out.                                                                                                                                                             | `1`                            |
| `jellyfin.readinessProbe.enabled`               | Whether to enable the readiness probe.                                                                                                                                                                             | `false`                        |
| `jellyfin.readinessProbe.failureThreshold`      | The number of times to retry before giving up.                                                                                                                                                                     | `3`                            |
| `jellyfin.readinessProbe.initialDelaySeconds`   | The number of seconds to wait before starting the probe.                                                                                                                                                           | `10`                           |
| `jellyfin.readinessProbe.periodSeconds`         | The number of seconds between probe attempts.                                                                                                                                                                      | `10`                           |
| `jellyfin.readinessProbe.successThreshold`      | The minimum consecutive successes required to consider the probe successful.                                                                                                                                       | `1`                            |
| `jellyfin.readinessProbe.timeoutSeconds`        | The number of seconds after which the probe times out.                                                                                                                                                             | `1`                            |
| `jellyfin.tailscale.enabled`                    | Whether to enable tailscale integration.                                                                                                                                                                           | `false`                        |
| `jellyfin.tailscale.sidecar.enabled`            | Whether to enable sidecar container mode. Requires the tailscale auth key.                                                                                                                                         | `true`                         |
| `jellyfin.tailscale.sidecar.authKey`            | The tailscale auth key to use for the sidecar container.                                                                                                                                                           | `""`                           |
| `jellyfin.tailscale.sidecar.existingAuthSecret` | The name of an existing secret containing the tailscale auth key as TS_AUTHKEY.                                                                                                                                    | `""`                           |
| `jellyfin.tailscale.ingress.enabled`            | Whether to enable ingress mode. Requires the tailscale operator to be preinstalled and is required when enableDLNA is true.                                                                                        | `false`                        |
| `jellyfin.tailscale.ingress.host`               | The hostname of the tailscale node. Uses magicDNS, requires it to be enabled along with HTTPS on the tailscale dashboard.                                                                                          | `""`                           |

### Sonarr parameters

| Name                                                   | Description                                                                                                                         | Value                        |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `sonarr.enabled`                                       | Whether to enable Sonarr.                                                                                                           | `true`                       |
| `sonarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                          |
| `sonarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/sonarr` |
| `sonarr.image.tag`                                     | The image tag to use.                                                                                                               | `4.0.10`                     |
| `sonarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`               |
| `sonarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                         |
| `sonarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                   |
| `sonarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                       |
| `sonarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                         |
| `sonarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                         |
| `sonarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                         |
| `sonarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                         |
| `sonarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                         |
| `sonarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                         |
| `sonarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                  |
| `sonarr.service.port`                                  | The port on which the service will run.                                                                                             | `80`                         |
| `sonarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                         |
| `sonarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                      |
| `sonarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                         |
| `sonarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                         |
| `sonarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`        |
| `sonarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                          |
| `sonarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`     |
| `sonarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                         |
| `sonarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                         |
| `sonarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                      |
| `sonarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                          |
| `sonarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                        |
| `sonarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                         |
| `sonarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                         |
| `sonarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                         |
| `sonarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                         |
| `sonarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                         |
| `sonarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                       |
| `sonarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                       |
| `sonarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`              |
| `sonarr.env.UMASK`                                     | The umask to use for the pod.                                                                                                       | `002`                        |
| `sonarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                       |
| `sonarr.persistence.path`                              | The path to use for the persistence. Don't use slashes.                                                                             | `tv`                         |
| `sonarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                         |
| `sonarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                         |
| `sonarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`              |
| `sonarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                      |
| `sonarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                         |
| `sonarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                         |

### qBittorrent parameters

| Name                                                        | Description                                                                                                                         | Value                             |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| `qbittorrent.enabled`                                       | Whether to enable qBittorrent.                                                                                                      | `true`                            |
| `qbittorrent.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                               |
| `qbittorrent.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/qbittorrent` |
| `qbittorrent.image.tag`                                     | The image tag to use.                                                                                                               | `4.6.7`                           |
| `qbittorrent.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`                    |
| `qbittorrent.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                              |
| `qbittorrent.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                        |
| `qbittorrent.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                            |
| `qbittorrent.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                              |
| `qbittorrent.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                              |
| `qbittorrent.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                              |
| `qbittorrent.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                              |
| `qbittorrent.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                              |
| `qbittorrent.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                              |
| `qbittorrent.service.web.type`                              | The type of service to create.                                                                                                      | `ClusterIP`                       |
| `qbittorrent.service.web.port`                              | The port on which the service will run.                                                                                             | `8080`                            |
| `qbittorrent.service.web.nodePort`                          | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                              |
| `qbittorrent.service.bt.type`                               | The type of service to create.                                                                                                      | `ClusterIP`                       |
| `qbittorrent.service.bt.port`                               | The port on which the service will run.                                                                                             | `6881`                            |
| `qbittorrent.service.bt.nodePort`                           | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                              |
| `qbittorrent.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                           |
| `qbittorrent.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                              |
| `qbittorrent.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                              |
| `qbittorrent.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`             |
| `qbittorrent.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                               |
| `qbittorrent.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`          |
| `qbittorrent.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                              |
| `qbittorrent.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                              |
| `qbittorrent.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                           |
| `qbittorrent.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                               |
| `qbittorrent.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                             |
| `qbittorrent.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                              |
| `qbittorrent.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                              |
| `qbittorrent.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                              |
| `qbittorrent.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                              |
| `qbittorrent.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                              |
| `qbittorrent.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                            |
| `qbittorrent.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                            |
| `qbittorrent.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`                   |
| `qbittorrent.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                            |
| `qbittorrent.persistence.path`                              | The path to use for the persistence. Don't use slashes.                                                                             | `downloads`                       |
| `qbittorrent.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                              |
| `qbittorrent.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                              |
| `qbittorrent.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`                   |
| `qbittorrent.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                           |
| `qbittorrent.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                              |
| `qbittorrent.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                              |

### Prowlarr parameters

| Name                                                     | Description                                                                                                                         | Value                          |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `prowlarr.enabled`                                       | Whether to enable Prowlarr.                                                                                                         | `true`                         |
| `prowlarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                            |
| `prowlarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/prowlarr` |
| `prowlarr.image.tag`                                     | The image tag to use.                                                                                                               | `1.25.4`                       |
| `prowlarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`                 |
| `prowlarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                           |
| `prowlarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                     |
| `prowlarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                         |
| `prowlarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                           |
| `prowlarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                           |
| `prowlarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                           |
| `prowlarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                           |
| `prowlarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                           |
| `prowlarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                           |
| `prowlarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                    |
| `prowlarr.service.port`                                  | The port on which the service will run.                                                                                             | `9696`                         |
| `prowlarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                           |
| `prowlarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                        |
| `prowlarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                           |
| `prowlarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                           |
| `prowlarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`          |
| `prowlarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                            |
| `prowlarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`       |
| `prowlarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                           |
| `prowlarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                           |
| `prowlarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                        |
| `prowlarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                            |
| `prowlarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                          |
| `prowlarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                           |
| `prowlarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                           |
| `prowlarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                           |
| `prowlarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                           |
| `prowlarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                           |
| `prowlarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                         |
| `prowlarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                         |
| `prowlarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`                |
| `prowlarr.env.UMASK`                                     | The umask to use for the pod.                                                                                                       | `002`                          |
| `prowlarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                         |
| `prowlarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                           |
| `prowlarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                           |
| `prowlarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`                |
| `prowlarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                        |
| `prowlarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                           |
| `prowlarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                           |

### FlareSolverr parameters

| Name                                                         | Description                                                                                                                         | Value                               |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `flaresolverr.enabled`                                       | Whether to enable FlareSolverr.                                                                                                     | `true`                              |
| `flaresolverr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                                 |
| `flaresolverr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `ghcr.io/flaresolverr/flaresolverr` |
| `flaresolverr.image.tag`                                     | The image tag to use.                                                                                                               | `v3.3.21`                           |
| `flaresolverr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`                      |
| `flaresolverr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                                |
| `flaresolverr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                          |
| `flaresolverr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                              |
| `flaresolverr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                                |
| `flaresolverr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                                |
| `flaresolverr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                                |
| `flaresolverr.podSecurityContext.fsGroup`                    | The group ID to use for the pod.                                                                                                    | `1000`                              |
| `flaresolverr.podSecurityContext.fsGroupChangePolicy`        | The policy to use for the pod.                                                                                                      | `OnRootMismatch`                    |
| `flaresolverr.securityContext.allowPrivilegeEscalation`      | Whether to allow privilege escalation.                                                                                              | `false`                             |
| `flaresolverr.securityContext.capabilities.drop`             | The capabilities to drop.                                                                                                           | `["ALL"]`                           |
| `flaresolverr.securityContext.readOnlyRootFilesystem`        | Whether to use a read-only root filesystem.                                                                                         | `false`                             |
| `flaresolverr.securityContext.runAsNonRoot`                  | Whether to run as a non-root user.                                                                                                  | `true`                              |
| `flaresolverr.securityContext.privileged`                    | Whether to run in privileged mode.                                                                                                  | `false`                             |
| `flaresolverr.securityContext.runAsUser`                     | The user ID to use for the container.                                                                                               | `1000`                              |
| `flaresolverr.securityContext.runAsGroup`                    | The group ID to use for the container.                                                                                              | `1000`                              |
| `flaresolverr.securityContext.seccompProfile.type`           | The type of seccomp profile to use.                                                                                                 | `RuntimeDefault`                    |
| `flaresolverr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                                |
| `flaresolverr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                         |
| `flaresolverr.service.port`                                  | The port on which the service will run.                                                                                             | `8191`                              |
| `flaresolverr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                                |
| `flaresolverr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                             |
| `flaresolverr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                                |
| `flaresolverr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                                |
| `flaresolverr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`               |
| `flaresolverr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                                 |
| `flaresolverr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`            |
| `flaresolverr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                                |
| `flaresolverr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                                |
| `flaresolverr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                             |
| `flaresolverr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                                 |
| `flaresolverr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                               |
| `flaresolverr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                                |
| `flaresolverr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                                |
| `flaresolverr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                                |
| `flaresolverr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                                |
| `flaresolverr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                                |

### Jellyseerr parameters

| Name                                                       | Description                                                                                                                         | Value                              |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| `jellyseerr.enabled`                                       | Whether to enable Jellyseerr.                                                                                                       | `true`                             |
| `jellyseerr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                                |
| `jellyseerr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `docker.io/fallenbagel/jellyseerr` |
| `jellyseerr.image.tag`                                     | The image tag to use.                                                                                                               | `2.0.1`                            |
| `jellyseerr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`                     |
| `jellyseerr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                               |
| `jellyseerr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                         |
| `jellyseerr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                             |
| `jellyseerr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                               |
| `jellyseerr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                               |
| `jellyseerr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                               |
| `jellyseerr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                               |
| `jellyseerr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                               |
| `jellyseerr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                               |
| `jellyseerr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                        |
| `jellyseerr.service.port`                                  | The port on which the service will run.                                                                                             | `5055`                             |
| `jellyseerr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                               |
| `jellyseerr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                            |
| `jellyseerr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                               |
| `jellyseerr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                               |
| `jellyseerr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`              |
| `jellyseerr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                                |
| `jellyseerr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`           |
| `jellyseerr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                               |
| `jellyseerr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                               |
| `jellyseerr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                            |
| `jellyseerr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                                |
| `jellyseerr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                              |
| `jellyseerr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                               |
| `jellyseerr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                               |
| `jellyseerr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                               |
| `jellyseerr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                               |
| `jellyseerr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                               |
| `jellyseerr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                             |
| `jellyseerr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                               |
| `jellyseerr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                               |
| `jellyseerr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`                    |
| `jellyseerr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                            |
| `jellyseerr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                               |
| `jellyseerr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                               |

### Bazarr parameters

| Name                                                   | Description                                                                                                                         | Value                        |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `bazarr.enabled`                                       | Whether to enable Bazarr.                                                                                                           | `true`                       |
| `bazarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                          |
| `bazarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/bazarr` |
| `bazarr.image.tag`                                     | The image tag to use.                                                                                                               | `1.4.5`                      |
| `bazarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`               |
| `bazarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                         |
| `bazarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                   |
| `bazarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                       |
| `bazarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                         |
| `bazarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                         |
| `bazarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                         |
| `bazarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                         |
| `bazarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                         |
| `bazarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                         |
| `bazarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                  |
| `bazarr.service.port`                                  | The port on which the service will run.                                                                                             | `6767`                       |
| `bazarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                         |
| `bazarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                      |
| `bazarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                         |
| `bazarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                         |
| `bazarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`        |
| `bazarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                          |
| `bazarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`     |
| `bazarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                         |
| `bazarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                         |
| `bazarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                      |
| `bazarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                          |
| `bazarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                        |
| `bazarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                         |
| `bazarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                         |
| `bazarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                         |
| `bazarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                         |
| `bazarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                         |
| `bazarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                       |
| `bazarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                       |
| `bazarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`              |
| `bazarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                       |
| `bazarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                         |
| `bazarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                         |
| `bazarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`              |
| `bazarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                      |
| `bazarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                         |
| `bazarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                         |

### Radarr parameters

| Name                                                   | Description                                                                                                                         | Value                        |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `radarr.enabled`                                       | Whether to enable Radarr.                                                                                                           | `true`                       |
| `radarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                          |
| `radarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/radarr` |
| `radarr.image.tag`                                     | The image tag to use.                                                                                                               | `5.14.0`                     |
| `radarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`               |
| `radarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                         |
| `radarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                   |
| `radarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                       |
| `radarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                         |
| `radarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                         |
| `radarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                         |
| `radarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                         |
| `radarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                         |
| `radarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                         |
| `radarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                  |
| `radarr.service.port`                                  | The port on which the service will run.                                                                                             | `7878`                       |
| `radarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                         |
| `radarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                      |
| `radarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                         |
| `radarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                         |
| `radarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`        |
| `radarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                          |
| `radarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`     |
| `radarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                         |
| `radarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                         |
| `radarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                      |
| `radarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                          |
| `radarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                        |
| `radarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                         |
| `radarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                         |
| `radarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                         |
| `radarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                         |
| `radarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                         |
| `radarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                       |
| `radarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                       |
| `radarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`              |
| `radarr.env.UMASK`                                     | The umask to use for the pod.                                                                                                       | `002`                        |
| `radarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                       |
| `radarr.persistence.path`                              | The path to use for the persistence. Don't use slashes.                                                                             | `movies`                     |
| `radarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                         |
| `radarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                         |
| `radarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`              |
| `radarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                      |
| `radarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                         |
| `radarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                         |

### Lidarr parameters

| Name                                                   | Description                                                                                                                         | Value                        |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `lidarr.enabled`                                       | Whether to enable Lidarr.                                                                                                           | `true`                       |
| `lidarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                          |
| `lidarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/lidarr` |
| `lidarr.image.tag`                                     | The image tag to use.                                                                                                               | `2.7.1`                      |
| `lidarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`               |
| `lidarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                         |
| `lidarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                   |
| `lidarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                       |
| `lidarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                         |
| `lidarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                         |
| `lidarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                         |
| `lidarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                         |
| `lidarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                         |
| `lidarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                         |
| `lidarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                  |
| `lidarr.service.port`                                  | The port on which the service will run.                                                                                             | `8686`                       |
| `lidarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                         |
| `lidarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                      |
| `lidarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                         |
| `lidarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                         |
| `lidarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`        |
| `lidarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                          |
| `lidarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`     |
| `lidarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                         |
| `lidarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                         |
| `lidarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                      |
| `lidarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                          |
| `lidarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                        |
| `lidarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                         |
| `lidarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                         |
| `lidarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                         |
| `lidarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                         |
| `lidarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                         |
| `lidarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                       |
| `lidarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                       |
| `lidarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`              |
| `lidarr.env.UMASK`                                     | The umask to use for the pod.                                                                                                       | `002`                        |
| `lidarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                       |
| `lidarr.persistence.path`                              | The path to use for the persistence. Don't use slashes.                                                                             | `music`                      |
| `lidarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                         |
| `lidarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                         |
| `lidarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`              |
| `lidarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                      |
| `lidarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                         |
| `lidarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                         |

### Readarr parameters

| Name                                                    | Description                                                                                                                         | Value                         |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `readarr.enabled`                                       | Whether to enable Readarr.                                                                                                          | `true`                        |
| `readarr.replicaCount`                                  | The number of replicas to deploy.                                                                                                   | `1`                           |
| `readarr.image.repository`                              | The Docker repository to pull the image from.                                                                                       | `lscr.io/linuxserver/readarr` |
| `readarr.image.tag`                                     | The image tag to use.                                                                                                               | `0.4.2-develop`               |
| `readarr.image.pullPolicy`                              | The logic of image pulling.                                                                                                         | `IfNotPresent`                |
| `readarr.imagePullSecrets`                              | The image pull secrets to use.                                                                                                      | `[]`                          |
| `readarr.deployment.strategy.type`                      | The deployment strategy to use.                                                                                                     | `Recreate`                    |
| `readarr.serviceAccount.create`                         | Whether to create a service account.                                                                                                | `true`                        |
| `readarr.serviceAccount.annotations`                    | Additional annotations to add to the service account.                                                                               | `{}`                          |
| `readarr.serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                          |
| `readarr.podAnnotations`                                | Additional annotations to add to the pod.                                                                                           | `{}`                          |
| `readarr.podSecurityContext`                            | The security context to use for the pod.                                                                                            | `{}`                          |
| `readarr.securityContext`                               | The security context to use for the container.                                                                                      | `{}`                          |
| `readarr.initContainers`                                | Additional init containers to add to the pod.                                                                                       | `[]`                          |
| `readarr.service.type`                                  | The type of service to create.                                                                                                      | `ClusterIP`                   |
| `readarr.service.port`                                  | The port on which the service will run.                                                                                             | `8787`                        |
| `readarr.service.nodePort`                              | The nodePort to use for the service. Only used if service.type is NodePort.                                                         | `""`                          |
| `readarr.ingress.enabled`                               | Whether to create an ingress for the service.                                                                                       | `false`                       |
| `readarr.ingress.className`                             | The ingress class name to use.                                                                                                      | `""`                          |
| `readarr.ingress.annotations`                           | Additional annotations to add to the ingress.                                                                                       | `{}`                          |
| `readarr.ingress.hosts[0].host`                         | The host to use for the ingress.                                                                                                    | `chart-example.local`         |
| `readarr.ingress.hosts[0].paths[0].path`                | The path to use for the ingress.                                                                                                    | `/`                           |
| `readarr.ingress.hosts[0].paths[0].pathType`            | The path type to use for the ingress.                                                                                               | `ImplementationSpecific`      |
| `readarr.ingress.tls`                                   | The TLS configuration for the ingress.                                                                                              | `[]`                          |
| `readarr.resources`                                     | The resources to use for the pod.                                                                                                   | `{}`                          |
| `readarr.autoscaling.enabled`                           | Whether to enable autoscaling.                                                                                                      | `false`                       |
| `readarr.autoscaling.minReplicas`                       | The minimum number of replicas to scale to.                                                                                         | `1`                           |
| `readarr.autoscaling.maxReplicas`                       | The maximum number of replicas to scale to.                                                                                         | `100`                         |
| `readarr.autoscaling.targetCPUUtilizationPercentage`    | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                          |
| `readarr.autoscaling.targetMemoryUtilizationPercentage` | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                          |
| `readarr.nodeSelector`                                  | The node selector to use for the pod.                                                                                               | `{}`                          |
| `readarr.tolerations`                                   | The tolerations to use for the pod.                                                                                                 | `[]`                          |
| `readarr.affinity`                                      | The affinity to use for the pod.                                                                                                    | `{}`                          |
| `readarr.env.PUID`                                      | The user ID to use for the pod.                                                                                                     | `1000`                        |
| `readarr.env.PGID`                                      | The group ID to use for the pod.                                                                                                    | `1000`                        |
| `readarr.env.TZ`                                        | The timezone to use for the pod.                                                                                                    | `Europe/London`               |
| `readarr.env.UMASK`                                     | The umask to use for the pod.                                                                                                       | `002`                         |
| `readarr.persistence.enabled`                           | Whether to enable persistence.                                                                                                      | `true`                        |
| `readarr.persistence.path`                              | The path to use for the persistence. Don't use slashes.                                                                             | `books`                       |
| `readarr.persistence.storageClass`                      | The storage class to use for the persistence.                                                                                       | `""`                          |
| `readarr.persistence.existingClaim`                     | The name of an existing claim to use for the persistence.                                                                           | `""`                          |
| `readarr.persistence.accessMode`                        | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`               |
| `readarr.persistence.size`                              | The size to use for the persistence.                                                                                                | `800Mi`                       |
| `readarr.persistence.additionalVolumes`                 | Additional volumes to add to the pod.                                                                                               | `[]`                          |
| `readarr.persistence.additionalMounts`                  | Additional volume mounts to add to the pod.                                                                                         | `[]`                          |

### Cleanuperr parameters

| Name                                                             | Description                                                                                                                         | Value                                                                                      |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `cleanuperr.enabled`                                             | Whether to enable Cleanuperr.                                                                                                       | `false`                                                                                    |
| `cleanuperr.replicaCount`                                        | The number of replicas to deploy.                                                                                                   | `1`                                                                                        |
| `cleanuperr.image.repository`                                    | The Docker repository to pull the image from.                                                                                       | `ghcr.io/flmorg/cleanuperr`                                                                |
| `cleanuperr.image.tag`                                           | The image tag to use.                                                                                                               | `latest`                                                                                   |
| `cleanuperr.image.pullPolicy`                                    | The logic of image pulling.                                                                                                         | `IfNotPresent`                                                                             |
| `cleanuperr.imagePullSecrets`                                    | The image pull secrets to use.                                                                                                      | `[]`                                                                                       |
| `cleanuperr.deployment.strategy.type`                            | The deployment strategy to use.                                                                                                     | `Recreate`                                                                                 |
| `cleanuperr.serviceAccount.create`                               | Whether to create a service account.                                                                                                | `true`                                                                                     |
| `cleanuperr.serviceAccount.annotations`                          | Additional annotations to add to the service account.                                                                               | `{}`                                                                                       |
| `cleanuperr.serviceAccount.name`                                 | The name of the service account to use. If not set and create is true, a new service account will be created with a generated name. | `""`                                                                                       |
| `cleanuperr.podAnnotations`                                      | Additional annotations to add to the pod.                                                                                           | `{}`                                                                                       |
| `cleanuperr.podSecurityContext`                                  | The security context to use for the pod.                                                                                            | `{}`                                                                                       |
| `cleanuperr.securityContext`                                     | The security context to use for the container.                                                                                      | `{}`                                                                                       |
| `cleanuperr.initContainers`                                      | Additional init containers to add to the pod.                                                                                       | `[]`                                                                                       |
| `cleanuperr.resources`                                           | The resources to use for the pod.                                                                                                   | `{}`                                                                                       |
| `cleanuperr.autoscaling.enabled`                                 | Whether to enable autoscaling.                                                                                                      | `false`                                                                                    |
| `cleanuperr.autoscaling.minReplicas`                             | The minimum number of replicas to scale to.                                                                                         | `1`                                                                                        |
| `cleanuperr.autoscaling.maxReplicas`                             | The maximum number of replicas to scale to.                                                                                         | `100`                                                                                      |
| `cleanuperr.autoscaling.targetCPUUtilizationPercentage`          | The target CPU utilization percentage to use for autoscaling.                                                                       | `80`                                                                                       |
| `cleanuperr.autoscaling.targetMemoryUtilizationPercentage`       | The target memory utilization percentage to use for autoscaling.                                                                    | `80`                                                                                       |
| `cleanuperr.nodeSelector`                                        | The node selector to use for the pod.                                                                                               | `{}`                                                                                       |
| `cleanuperr.tolerations`                                         | The tolerations to use for the pod.                                                                                                 | `[]`                                                                                       |
| `cleanuperr.affinity`                                            | The affinity to use for the pod.                                                                                                    | `{}`                                                                                       |
| `cleanuperr.secrets.existingSecret`                              | The name of an existing Secret to use for credentials instead of creating a new one.                                                | `""`                                                                                       |
| `cleanuperr.secrets.qbittorrent.username`                        | The qBittorrent username to be stored in the secret.                                                                                | `""`                                                                                       |
| `cleanuperr.secrets.qbittorrent.password`                        | The qBittorrent password to be stored in the secret.                                                                                | `""`                                                                                       |
| `cleanuperr.secrets.sonarr.apiKey`                               | The Sonarr API key to be stored in the secret.                                                                                      | `""`                                                                                       |
| `cleanuperr.secrets.radarr.apiKey`                               | The Radarr API key to be stored in the secret.                                                                                      | `""`                                                                                       |
| `cleanuperr.secrets.lidarr.apiKey`                               | The Lidarr API key to be stored in the secret.                                                                                      | `""`                                                                                       |
| `cleanuperr.env.TZ`                                              | The timezone to use for the pod.                                                                                                    | `Europe/London`                                                                            |
| `cleanuperr.env.DRY_RUN`                                         | Enable dry run mode to log actions without actually performing them.                                                                | `false`                                                                                    |
| `cleanuperr.env.LOGGING__LOGLEVEL`                               | The log level for the application.                                                                                                  | `Information`                                                                              |
| `cleanuperr.env.LOGGING__FILE__ENABLED`                          | Whether to enable file logging.                                                                                                     | `true`                                                                                     |
| `cleanuperr.env.LOGGING__FILE__PATH`                             | The path to store log files.                                                                                                        | `/var/logs`                                                                                |
| `cleanuperr.env.LOGGING__ENHANCED`                               | Whether to enable enhanced logging.                                                                                                 | `true`                                                                                     |
| `cleanuperr.env.HTTP_MAX_RETRIES`                                | The maximum number of HTTP retries.                                                                                                 | `3`                                                                                        |
| `cleanuperr.env.HTTP_TIMEOUT`                                    | The HTTP timeout in seconds.                                                                                                        | `100`                                                                                      |
| `cleanuperr.env.HTTP_VALIDATE_CERT`                              | Whether to validate HTTPS certificates.                                                                                             | `Enabled`                                                                                  |
| `cleanuperr.env.SEARCH_ENABLED`                                  | Whether to enable search functionality.                                                                                             | `true`                                                                                     |
| `cleanuperr.env.SEARCH_DELAY`                                    | The delay between searches in seconds.                                                                                              | `30`                                                                                       |
| `cleanuperr.env.QUEUECLEANER__ENABLED`                           | Whether to enable queue cleaner functionality.                                                                                      | `true`                                                                                     |
| `cleanuperr.env.TRIGGERS__QUEUECLEANER`                          | The cron schedule for queue cleaner.                                                                                                | `0 0/5 * * * ?`                                                                            |
| `cleanuperr.env.QUEUECLEANER__IGNORED_DOWNLOADS_PATH`            | Path to ignored downloads list. Set to /ignored to use the default.                                                                 | `/ignored/ignored_downloads`                                                               |
| `cleanuperr.env.QUEUECLEANER__RUNSEQUENTIALLY`                   | Whether to run queue cleaner sequentially.                                                                                          | `true`                                                                                     |
| `cleanuperr.env.QUEUECLEANER__IMPORT_FAILED_MAX_STRIKES`         | Maximum failed import strikes before action.                                                                                        | `0`                                                                                        |
| `cleanuperr.env.QUEUECLEANER__IMPORT_FAILED_IGNORE_PRIVATE`      | Whether to ignore private trackers for failed imports.                                                                              | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__IMPORT_FAILED_DELETE_PRIVATE`      | Whether to delete private tracker torrents on failed imports.                                                                       | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__IMPORT_FAILED_IGNORE_PATTERNS__0`  | Patterns to ignore for failed imports.                                                                                              | `title mismatch`                                                                           |
| `cleanuperr.env.QUEUECLEANER__IMPORT_FAILED_IGNORE_PATTERNS__1`  | Additional patterns to ignore for failed imports.                                                                                   | `manual import required`                                                                   |
| `cleanuperr.env.QUEUECLEANER__STALLED_MAX_STRIKES`               | Maximum stalled strikes before action.                                                                                              | `0`                                                                                        |
| `cleanuperr.env.QUEUECLEANER__STALLED_RESET_STRIKES_ON_PROGRESS` | Whether to reset strikes on progress for stalled downloads.                                                                         | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__STALLED_IGNORE_PRIVATE`            | Whether to ignore private trackers for stalled torrents.                                                                            | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__STALLED_DELETE_PRIVATE`            | Whether to delete private tracker torrents when stalled.                                                                            | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__DOWNLOADING_METADATA_MAX_STRIKES`  | Maximum strikes for downloading metadata.                                                                                           | `0`                                                                                        |
| `cleanuperr.env.QUEUECLEANER__SLOW_MAX_STRIKES`                  | Maximum slow download strikes before action.                                                                                        | `0`                                                                                        |
| `cleanuperr.env.QUEUECLEANER__SLOW_RESET_STRIKES_ON_PROGRESS`    | Whether to reset strikes on progress for slow downloads.                                                                            | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__SLOW_IGNORE_PRIVATE`               | Whether to ignore private trackers for slow downloads.                                                                              | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__SLOW_DELETE_PRIVATE`               | Whether to delete private tracker torrents when slow.                                                                               | `false`                                                                                    |
| `cleanuperr.env.QUEUECLEANER__SLOW_MIN_SPEED`                    | Minimum speed threshold for slow downloads.                                                                                         | `""`                                                                                       |
| `cleanuperr.env.QUEUECLEANER__SLOW_MAX_TIME`                     | Maximum time allowed for slow downloads.                                                                                            | `0`                                                                                        |
| `cleanuperr.env.QUEUECLEANER__SLOW_IGNORE_ABOVE_SIZE`            | Ignore torrents above this size for slow download checks.                                                                           | `""`                                                                                       |
| `cleanuperr.env.CONTENTBLOCKER__ENABLED`                         | Whether to enable content blocker functionality.                                                                                    | `true`                                                                                     |
| `cleanuperr.env.TRIGGERS__CONTENTBLOCKER`                        | Cron schedule for content blocker.                                                                                                  | `0 0/5 * * * ?`                                                                            |
| `cleanuperr.env.CONTENTBLOCKER__IGNORED_DOWNLOADS_PATH`          | Path to ignored downloads for content blocker. Set to /ignored to use the default.                                                  | `/ignored/ignored_downloads`                                                               |
| `cleanuperr.env.CONTENTBLOCKER__IGNORE_PRIVATE`                  | Whether to ignore private trackers for content blocking.                                                                            | `false`                                                                                    |
| `cleanuperr.env.CONTENTBLOCKER__DELETE_PRIVATE`                  | Whether to delete private tracker torrents when blocked.                                                                            | `false`                                                                                    |
| `cleanuperr.env.DOWNLOADCLEANER__ENABLED`                        | Whether to enable download cleaner functionality.                                                                                   | `true`                                                                                     |
| `cleanuperr.env.TRIGGERS__DOWNLOADCLEANER`                       | Cron schedule for download cleaner.                                                                                                 | `0 0 * * * ?`                                                                              |
| `cleanuperr.env.DOWNLOADCLEANER__IGNORED_DOWNLOADS_PATH`         | Path to ignored downloads for download cleaner. Set to /ignored to use the default.                                                 | `/ignored/ignored_downloads`                                                               |
| `cleanuperr.env.DOWNLOADCLEANER__DELETE_PRIVATE`                 | Whether to delete private tracker torrents when cleaning.                                                                           | `false`                                                                                    |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__0__NAME`            | Name for the first category (Sonarr).                                                                                               | `tv-sonarr`                                                                                |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__1__NAME`            | Name for the second category (Radarr).                                                                                              | `movies-radarr`                                                                            |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__2__NAME`            | Name for the third category (Lidarr).                                                                                               | `music-lidarr`                                                                             |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__0__MAX_RATIO`       | Max ratio for the first category.                                                                                                   | `1.0`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__1__MAX_RATIO`       | Max ratio for the second category.                                                                                                  | `1.0`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__2__MAX_RATIO`       | Max ratio for the third category.                                                                                                   | `1.0`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__0__MIN_SEED_TIME`   | Min seed time for the first category in minutes.                                                                                    | `360`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__1__MIN_SEED_TIME`   | Min seed time for the second category in minutes.                                                                                   | `360`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__2__MIN_SEED_TIME`   | Min seed time for the third category in minutes.                                                                                    | `360`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__0__MAX_SEED_TIME`   | Max seed time for the first category in minutes.                                                                                    | `720`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__1__MAX_SEED_TIME`   | Max seed time for the second category in minutes.                                                                                   | `720`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__CATEGORIES__2__MAX_SEED_TIME`   | Max seed time for the third category in minutes.                                                                                    | `720`                                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_TARGET_CATEGORY`       | Target category for unlinked downloads.                                                                                             | `cleanuperr-unlinked`                                                                      |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_USE_TAG`               | Whether to use a tag for unlinked downloads.                                                                                        | `false`                                                                                    |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_IGNORED_ROOT_DIR`      | Root directory to ignore for unlinked downloads.                                                                                    | `/media/downloads`                                                                         |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_CATEGORIES__0`         | First category to check for unlinked downloads.                                                                                     | `tv-sonarr`                                                                                |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_CATEGORIES__1`         | Second category to check for unlinked downloads.                                                                                    | `movies-radarr`                                                                            |
| `cleanuperr.env.DOWNLOADCLEANER__UNLINKED_CATEGORIES__2`         | Third category to check for unlinked downloads.                                                                                     | `music-lidarr`                                                                             |
| `cleanuperr.env.DOWNLOAD_CLIENT`                                 | The download client to use.                                                                                                         | `qbittorrent`                                                                              |
| `cleanuperr.env.QBITTORRENT__URL`                                | The URL for qBittorrent web interface.                                                                                              | `http://servarr-qbittorrent-web:8080`                                                      |
| `cleanuperr.env.QBITTORRENT__URL_BASE`                           | The base URL for qBittorrent.                                                                                                       | `""`                                                                                       |
| `cleanuperr.env.SONARR__ENABLED`                                 | Whether to enable Sonarr integration.                                                                                               | `true`                                                                                     |
| `cleanuperr.env.SONARR__IMPORT_FAILED_MAX_STRIKES`               | Maximum failed import strikes for Sonarr.                                                                                           | `-1`                                                                                       |
| `cleanuperr.env.SONARR__BLOCK__TYPE`                             | The block type for Sonarr.                                                                                                          | `blacklist`                                                                                |
| `cleanuperr.env.SONARR__BLOCK__PATH`                             | Path to the Sonarr blocklist.                                                                                                       | `https://raw.githubusercontent.com/flmorg/cleanuperr/refs/heads/main/blacklist_permissive` |
| `cleanuperr.env.SONARR__SEARCHTYPE`                              | The search type for Sonarr.                                                                                                         | `Episode`                                                                                  |
| `cleanuperr.env.SONARR__INSTANCES__0__URL`                       | The URL for Sonarr.                                                                                                                 | `http://servarr-sonarr:80`                                                                 |
| `cleanuperr.env.RADARR__ENABLED`                                 | Whether to enable Radarr integration.                                                                                               | `true`                                                                                     |
| `cleanuperr.env.RADARR__IMPORT_FAILED_MAX_STRIKES`               | Maximum failed import strikes for Radarr.                                                                                           | `-1`                                                                                       |
| `cleanuperr.env.RADARR__BLOCK__TYPE`                             | The block type for Radarr.                                                                                                          | `blacklist`                                                                                |
| `cleanuperr.env.RADARR__BLOCK__PATH`                             | Path to the Radarr blocklist.                                                                                                       | `https://raw.githubusercontent.com/flmorg/cleanuperr/refs/heads/main/blacklist_permissive` |
| `cleanuperr.env.RADARR__INSTANCES__0__URL`                       | The URL for Radarr.                                                                                                                 | `http://servarr-radarr:7878`                                                               |
| `cleanuperr.env.LIDARR__ENABLED`                                 | Whether to enable Lidarr integration.                                                                                               | `false`                                                                                    |
| `cleanuperr.env.LIDARR__IMPORT_FAILED_MAX_STRIKES`               | Maximum failed import strikes for Lidarr.                                                                                           | `-1`                                                                                       |
| `cleanuperr.env.LIDARR__BLOCK__TYPE`                             | The block type for Lidarr.                                                                                                          | `blacklist`                                                                                |
| `cleanuperr.env.LIDARR__BLOCK__PATH`                             | Path to the Lidarr blocklist.                                                                                                       | `https://raw.githubusercontent.com/flmorg/cleanuperr/refs/heads/main/blacklist_permissive` |
| `cleanuperr.env.LIDARR__INSTANCES__0__URL`                       | The URL for Lidarr.                                                                                                                 | `http://servarr-lidarr:8686`                                                               |
| `cleanuperr.ignoredDownloads.entries`                            | List of torrent names or patterns to ignore in the ignored_downloads.txt ConfigMap.                                                 | `[]`                                                                                       |
| `cleanuperr.persistence.logs.enabled`                            | Whether to enable persistence.                                                                                                      | `false`                                                                                    |
| `cleanuperr.persistence.logs.storageClass`                       | The storage class to use for the persistence.                                                                                       | `""`                                                                                       |
| `cleanuperr.persistence.logs.existingClaim`                      | The name of an existing claim to use for the persistence.                                                                           | `""`                                                                                       |
| `cleanuperr.persistence.logs.accessMode`                         | The access mode to use for the persistence.                                                                                         | `ReadWriteOnce`                                                                            |
| `cleanuperr.persistence.logs.size`                               | The size to use for the persistence.                                                                                                | `100Mi`                                                                                    |
| `cleanuperr.persistence.logs.additionalVolumes`                  | Additional volumes to add to the pod.                                                                                               | `[]`                                                                                       |
| `cleanuperr.persistence.logs.additionalMounts`                   | Additional volume mounts to add to the pod.                                                                                         | `[]`                                                                                       |

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

## Configuration and helpful examples

### Shared volume and Sonarr hardlinks

The shared media volume is a shared volume between all the apps that need it. It is used to store the media files. It is created by Jellyfin.

Sonarr can use hardlinks to save space, but it needs to write to the same volume as the original files. This is where the shared media volume comes in. By design, all apps that use the shared media volume will have the same ownership and permissions, and the directories are created with init containers that set the ownership and permissions.

If you use subPath in the volumeMounts, Sonarr will not be able to create hardlinks because Kubernetes sees the subdirectory as a different filesystem and will not be able to create hardlinks.

It is highly recommended that you simply use the chart's default values for this. These are the directories that are mounted by Sonarr, Radarr, Lidarr, Readarr, and qBittorrent:

- `/media/tv`
- `/media/movies`
- `/media/music`
- `/media/books`
- `/media/downloads`

### Local path provisioner scenario

To make this chart work with a local path provisioner, you must deploy the whole stack on a single node. This is because `hostPath` does not support ReadWriteMany as storage access mode. Simply use a node selector and use the same storage class:

```yaml
jellyfin:
  nodeSelector:
    kubito/hdd: enabled

  persistence:
    config:
      enabled: true
      storageClass: hdd
      size: 1Gi
    media:
      enabled: true
      storageClass: hdd
      size: 100Gi
      accessMode: ReadWriteOnce
```

### Intro Skipper plugin for Jellyfin permissions fix

The Intro Skipper plugin for Jellyfin, which is really useful, will complain that it can't write to the `/usr/share/jellyfin/web/index.html` file inside the Jellyfin pod. To fix this, simply add an init container:

```yaml
jellyfin:
  extraVolumeMounts:
    - name: custom-cont-init
      mountPath: /custom-cont-init.d

  extraVolumes:
    - name: custom-cont-init
      emptyDir: {}

  extraInitContainers:
    - name: create-custom-init-script
      image: busybox
      command:
        - sh
        - -c
        - |
          cat << 'EOF' > /custom-cont-init.d/01-fix-permissions.sh
          #!/bin/sh
          chown abc /usr/share/jellyfin/web/index.html
          EOF
          chmod +x /custom-cont-init.d/01-fix-permissions.sh
      volumeMounts:
        - name: custom-cont-init
          mountPath: /custom-cont-init.d
```

### Tailscale

This chart supports Tailscale in two modes: ingress and sidecar. By default, it deploys in sidecar mode, it will create a secret containing the Tailscale auth key that you provide. If you already have a secret with the Tailscale auth key, you can use that by setting the `tailscale.sidecar.existingAuthSecret` value. It sets the correct permissions for the service account to be able to read the secret. Once Jellyfin runs, it will join your tailnet and you will be able to access the apps from any device on your tailnet.

If you want to use ingress mode, you need to have the Tailscale operator deployed in the same cluster and set the `tailscale.ingress.host` value to the ingress host you want to use. You need to enable MagicDNS and HTTPS in the Tailscale admin console to be able to use this mode. To deploy the Tailscale operator, see the [Tailscale operator documentation](https://tailscale.com/kb/1236/kubernetes-operator).

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

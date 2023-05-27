# Traefik Whitelist DDNS

This is a simple Traefik v2 DDNS updater script, which can be used in home lab setups to synchronize the allowed IP addresses for your [IP Whitelist Middleware](https://doc.traefik.io/traefik/middlewares/http/ipwhitelist/), which allows you to configure a VPN protected setup for your applications. It also support setting a custom domain for its IP(s) to be added to the whitelist range of the middleware.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install traefik-whitelist-ddns --namespace traefik kubitodev/traefik-whitelist-ddns
```

## Introduction

This chart can be used to solve the problem of a whitelisting a single IP address by using a Traefik Middleware, which can allow you to use your own VPN to protect services from external access. It also support setting a custom domain for its IP(s) to be added to the whitelist range of the middleware.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+
- The namespace should always be Traefik's namespace!

## Installing the Chart

To install the chart with the release name `traefik-whitelist-ddns`:

```console
helm install traefik-whitelist-ddns --namespace traefik kubitodev/traefik-whitelist-ddns
```

The command deploys traefik-whitelist-ddns on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `traefik-whitelist-ddns` deployment:

```console
helm delete traefik-whitelist-ddns
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Image parameters

| Name               | Description                                   | Value                                 |
| ------------------ | --------------------------------------------- | ------------------------------------- |
| `image.repository` | The Docker repository to pull the image from. | `kubitodev/traefik-ip-whitelist-sync` |
| `image.tag`        | The image tag to use.                         | `1.0.2`                               |

### Cron parameters

| Name                                  | Description                                                      | Value           |
| ------------------------------------- | ---------------------------------------------------------------- | --------------- |
| `cron.job.schedule`                   | Interval for how often the job should run.                       | `"*/5 * * * *"` |
| `cron.job.successfulJobsHistoryLimit` | Limit of the successful jobs saved in history as completed pods. | `1`             |
| `cron.pod.restartPolicy`              | The logic for restarting the cron pod.                           | `OnFailure`     |

### Middleware parameters

| Name                                | Description                                                                                                                               | Value          |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| `middleware.name`                   | The name of the IP whitelist middleware.                                                                                                  | `ip-whitelist` |
| `middleware.customDomain`           | The custom domain that you want to whitelist instead of your public IP.                                                                   | `""`           |
| `middleware.ipStrategy.enabled`     | The ipStrategy option defines two parameters that set how Traefik determines the client IP: depth, and excludedIPs.                       | `false`        |
| `middleware.ipStrategy.depth`       | The depth option tells Traefik to use the X-Forwarded-For header and take the IP located at the depth position (starting from the right). | `1`            |
| `middleware.ipStrategy.excludedIPs` | excludedIPs configures Traefik to scan the X-Forwarded-For header and select the first IP not in the list.                                | `[]`           |

### RBAC parameters

| Name                      | Description                                                             | Value     |
| ------------------------- | ----------------------------------------------------------------------- | --------- |
| `rbac.serviceAccountName` | The name of the default traefik service account in traefik's namespace. | `traefik` |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm install --namespace traefik example \
  --set user=example \
  --set password=example \
    kubitodev/example
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
helm install --namespace traefik example -f values.yaml kubitodev/example
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### Default namespace

This chart is intended to run where Traefik is installed, so always run it in that namespace, as it uses the Traefik service account to run the job.

### Custom domain

If you have your VPN on a custom domain and it is not a part of your network, you can set the `middleware.customDomain` value to that domain. It will get the IP(s) and whitelist them in the middleware as well, including your public IP so you can access it from home without VPN.

### Using ArgoCD

If you are using ArgoCD, the chart will not work if you don't set the ArgoCD application to ignore the `sourceRange` field of the `Middleware` CRD. So, in your application, set the following:

```yaml
ignoreDifferences:
    - group: traefik.containo.us
      kind: Middleware
      jsonPointers:
      - /spec/ipWhiteList/sourceRange
  syncPolicy:
    syncOptions:
      - RespectIgnoreDifferences=true
```

## Troubleshooting

### The IP is incorrect

If the IP you are getting in Traefik's logs is incorrect, and it is the one of the Traefik Load Balancer, try setting the `externalTrafficPolicy` of the Traefik's service to `Local`. For example, if you ran Traefik with Helm, set the `--set service.spec.externalTrafficPolicy=Local` flag.

## License

Copyright &copy; 2022 Kubito

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

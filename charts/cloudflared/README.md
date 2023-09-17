# Cloudflared

A tunneling daemon that proxies traffic from the Cloudflare network to your origins. This daemon sits between Cloudflare network and your origin (e.g. a webserver). Cloudflare attracts client requests and sends them to you via this daemon, without requiring you to poke holes on your firewall --- your origin can remain as closed as possible. Extensive documentation can be found in the [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps) section of the Cloudflare Docs.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install cloudflared kubitodev/cloudflared
```

## Introduction

Cloudflare Tunnel provides you with a secure way to connect your resources to Cloudflare without a publicly routable IP address. With Tunnel, you do not send traffic to an external IP — instead, a lightweight daemon in your infrastructure (cloudflared) creates outbound-only connections to Cloudflare’s edge. Cloudflare Tunnel can connect HTTP web servers, SSH servers, remote desktops, and other protocols safely to Cloudflare. This way, your origins can serve traffic through Cloudflare without being vulnerable to attacks that bypass Cloudflare.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+
- Argo Tunnel ID generated

## Installing the Chart

To install the chart with the release name `cloudflared`:

```console
helm install cloudflared kubitodev/cloudflared
```

The command deploys cloudflared on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `cloudflared` deployment:

```console
helm delete cloudflared
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Image parameters

| Name                    | Description                                   | Value                    |
| ----------------------- | --------------------------------------------- | ------------------------ |
| `image.repository`      | The Docker repository to pull the image from. | `cloudflare/cloudflared` |
| `image.tag`             | The image tag to use.                         | `2023.8.2`               |
| `image.imagePullPolicy` | The logic of image pulling.                   | `IfNotPresent`           |

### Deployment parameters

| Name                | Description                                                                  | Value   |
| ------------------- | ---------------------------------------------------------------------------- | ------- |
| `replicaCount`      | The number of replicas to deploy.                                            | `3`     |
| `tunnelID`          | The Argo Tunnel ID you created. Check the configuration section for details. | `""`    |
| `auth.accountTag`   | The Argo tunnel account tag.                                                 | `""`    |
| `auth.tunnelName`   | The Argo tunnel name.                                                        | `""`    |
| `auth.tunnelSecret` | The Argo tunnel secret.                                                      | `""`    |
| `existingSecret`    | The name of an existing secret containing the Argo tunnel settings.          | `""`    |
| `warpRouting`       | Whether to enable WARP traffic routing to local subnets.                     | `false` |
| `ingress`           | The ingress settings to apply. Check the configuration section for examples. | `[]`    |


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

### Getting the Argo Tunnel ID (required)

- Start by downloading and installing the lightweight Cloudflare Tunnel daemon, `cloudflared`. You can find it [here](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/).

- Once installed, you can use the tunnel login command in `cloudflared` to obtain a certificate:

```bash
cloudflared tunnel login
```

- Create the tunnel with:

```bash
cloudflared tunnel create example-tunnel
```

- Associate your tunnel with a CNAME DNS Record

```bash
cloudflared tunnel route dns example-tunnel tunnel.example.com
```

- The tunnel configuration can be found in `~/.cloudflared/<TUNNEL_ID>.json`. You will need it for creating a secret/configmap when deploying the Cloudflared instance on your cluster.

Now, when you want to create a new subdomain, just point it as a CNAME to the tunnel record, and it will be routed automatically!

For more information, check the [official guide](https://developers.cloudflare.com/cloudflare-one/tutorials/many-cfd-one-tunnel/).

### Setting up the Argo Tunnel ingress options with Traefik

To use the tunnel with Traefik, you need to configure the ingress settings. As cloudflared works with CNAMEs, you want to set a wildcard hostname for the service, and set the origin request setting to be the root domain that you are configuring this for. Also, you need to point the service to the secure port (443) of the Traefik load balancer service. Here is an example configuration:

```yaml
cloudflared:
  ingress:
    - hostname: "*.example.com"
      service: https://traefik.traefik-system.svc.cluster.local:443
      originRequest:
        originServerName: example.com
    - service: http_status:404
```

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

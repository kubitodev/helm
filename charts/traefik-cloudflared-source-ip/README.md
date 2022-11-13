# Traefik Cloudflared (Argo Tunnel) Source IP

A Traefik plugin chart which can be used when you are running a Cloudflared (Argo Tunnel) instance. It creates a middleware and uses an extended Traefik image which contains the plugin, and allows you to skip the Traefik Pilot setup.

> IMPORTANT: The usage of Argo Tunnels with Traefik requires you to generate a Cloudflare certificate and enable Full (strict) mode for SSL/TLS. More information can be found in the Configuration section of this page.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install traefik-cloudflared-source-ip kubitodev/traefik-cloudflared-source-ip
```

## Introduction

When Traefik runs behind Cloudflared, especially in case of a Kubernetes cluster which uses Traefik as a load balancer, it is unable to get the real source IP from which a request is coming from. The `X-Real-Ip` header, which Traefik uses instead of the usual `X-Forwarded-For` header, is always set to the IP of the Cloudflared service.

The plugin solves the issue by overwriting the `X-Real-Ip` header, as well the `X-Forwarded-For` header, to the value of the `Cf-Connecting-Ip` which is the real source IP and is set by the Cloudflared instance on each request.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+
- Traefik running with the `kubitodev/traefik-cloudflared-source-ip` extended image
- Cloudflared running with the Argo Tunnel ID ready
- Cloudflare generated TLS certificate and full (strict) mode set for SSL/TLS

## Installing the Chart

To install the chart with the release name `traefik-cloudflared-source-ip`:

```console
helm install traefik-cloudflared-source-ip kubitodev/traefik-cloudflared-source-ip
```

The command deploys traefik-cloudflared-source-ip on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `traefik-cloudflared-source-ip` deployment:

```console
helm delete traefik-cloudflared-source-ip
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Global parameters

| Name                        | Description                                                                                                                                                                                                                                                                                                         | Value  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `global.cloudflaredEnabled` | Whether to deploy the cloudflared subchart. It is required to have it, but you can deploy it on your own if you want. Check the [chart](https://artifacthub.io/packages/helm/kubitodev/cloudflared) for configuration.                                                                                              | `true` |
| `global.traefikEnabled`     | Whether to deploy the official Traefik chart as subchart. Check [the official chart](https://artifacthub.io/packages/helm/traefik/traefik) for configuration. IMPORTANT: If you want to deploy it on your own, make sure you use the `kubitodev/traefik-cloudflared-source-ip` image as the base image for Traefik. | `true` |


### Image parameters

| Name               | Description                                   | Value                                     |
| ------------------ | --------------------------------------------- | ----------------------------------------- |
| `image.repository` | The Docker repository to pull the image from. | `kubitodev/traefik-cloudflared-source-ip` |
| `image.tag`        | The image tag to use.                         | `1.0.6`                                   |


### Middleware parameters

| Name                      | Description                                                                                                                                  | Value                   |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `middleware.name`         | The name of the middleware.                                                                                                                  | `cloudflared-source-ip` |
| `middleware.excludedNets` | The source IP will be the first one that is not included in any of the CIDRs passed as the `excludedNets` parameter. Can be left as default. | `["1.1.1.1/24"]`        |


### [Traefik](https://artifacthub.io/packages/helm/traefik/traefik) parameters

| Name                                   | Description                                                                                                         | Value                                                                                                                 |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `traefik.image.repository`             | The Docker repository to pull the image from.                                                                       | `kubitodev/traefik-cloudflared-source-ip`                                                                             |
| `traefik.namespace`                    | The namespace to deploy Traefik to.                                                                                 | `traefik`                                                                                                             |
| `traefik.image.tag`                    | The image tag to use.                                                                                               | `1.0.6`                                                                                                               |
| `traefik.additionalArguments`          | Additional arguments to run the image with. The default one is required.                                            | `["--experimental.localPlugins.cloudflared-source-ip.moduleName=github.com/kubitodev/traefik-cloudflared-source-ip"]` |
| `traefik.cloudflareTls.existingSecret` | The name of an existing secret containing the `tls.crt` and `tls.key`.                                              | `""`                                                                                                                  |
| `traefik.cloudflareTls.crt`            | The TLS certificate obtained from Cloudflare. Check the configuration section for information on how to obtain one. | `""`                                                                                                                  |
| `traefik.cloudflareTls.key`            | The TLS key obtained from Cloudflare. Check the configuration section for information on how to obtain one.         | `""`                                                                                                                  |


### [Cloudflared](https://artifacthub.io/packages/helm/kubitodev/cloudflared) parameters

| Name                            | Description                                                                                            | Value                    |
| ------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------ |
| `cloudflared.image.repository`  | The Docker repository to pull the image from. Use `ghcr.io/milgradesec/cloudflared` for ARM64 devices. | `cloudflare/cloudflared` |
| `cloudflared.image.tag`         | The image tag to use.                                                                                  | `2022.10.3`              |
| `cloudflared.replicaCount`      | The number of replicas to deploy.                                                                      | `1`                      |
| `cloudflared.tunnelID`          | The Argo Tunnel ID you created. Check the configuration section for details.                           | `""`                     |
| `cloudflared.ingress`           | The ingress settings to apply. Check the configuration section for examples.                           | `[]`                     |
| `cloudflared.auth.accountTag`   | The Argo tunnel account tag.                                                                           | `""`                     |
| `cloudflared.auth.tunnelName`   | The Argo tunnel name.                                                                                  | `""`                     |
| `cloudflared.auth.tunnelSecret` | The Argo tunnel secret.                                                                                | `""`                     |
| `cloudflared.existingSecret`    | The name of an existing secret containing the Argo tunnel settings.                                    | `""`                     |


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

### Generating the SSL certificate (required)

Since the tunnel works with CNAME records, you will need to point the deployed instance of Cloudflared to Traefik's load balancer service which is your ingress controller. The catch is, you can't use your own TLS certificates because of how Cloudflared works with Traefik, so you'll have to generate the certificates via the Cloudflare dashboard. The good thing is, they last 15 years.

First, go to the [SSL/TLS Dashboard](https://dash.cloudflare.com/?to=/:account/:zone/ssl-tls) for your domain. Then, in the `Overview` tab, set the mode to `Full (strict)`. This means that it will require a mutual TLS connection between Cloudflare and the client (your Traefik instance). Then, go to the `Origin Server` tab, and click on `Create certificate`. Once there, choose the `Use my private key and CSR` option. Open a terminal and generate a CSR and Key with:

```bash
openssl req -new -newkey rsa:2048 -nodes -keyout tls.key -out tls.csr
```

Fill in the required fields, but most importantly set the `FDQN` value to your root domain, for example `example.com`. Copy the generated `tls.csr` and paste it in the input box on the Cloudflare page. Now, in the `Hostnames` field, enter your domain and a wildcard for subdomains, like `example.com` and `*.example.com`.

Finally, click on `Create`, and then download the generated certificate. Once you have it downloaded, rename it from `cert.pem` to `tls.crt`. Save the `tls.crt` and the previously generated `tls.key` as you will need them for creating the secret yourself, or for using them with this chart if you choose so. The secret will be used for creating a default TLS store in Traefik.

Go in the `SSL/TLS` dashboard again and select the `Edge Certificates` tab. Enable the `Always use HTTPS` option and that's it on the Cloudflare side.

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

### Setting up the Argo Tunnel ingress options

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

### Using the new default TLS store

The TLS store that is deployed with this chart uses the secret that contains the generated Cloudflare TLS certificate and key. To use it, just set the `tls: {}` field in your ingress route objects, and it will automatically choose to serve that certificate. Example:

```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: example-ingressroute
spec:
  ...
  tls: {}
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

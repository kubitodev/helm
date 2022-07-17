# Kubernetes Cloudflare DDNS

A simple Kubernetes cronjob which can be used for updating a DNS record on Cloudflare.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install example kubitodev/kubernetes-cloudflare-ddns
```

## Introduction

This chart can be used to solve the problem of a Kubernetes cluster being behind a Dynamic IP address and you need to configure a load balancer to run behind it. It is a good solution for home labs and similar setups.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `example`:

```console
helm install example kubitodev/kubernetes-cloudflare-ddns
```

The command deploys kubernetes-cloudflare-ddns on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `example` deployment:

```console
helm delete example
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Image parameters

| Name               | Description                                   | Value                                  |
| ------------------ | --------------------------------------------- | -------------------------------------- |
| `image.repository` | The Docker repository to pull the image from. | `kubitodev/kubernetes-cloudflare-ddns` |
| `image.tag`        | The image tag to use.                         | `1.0.1`                                |


### Cron parameters

| Name                                  | Description                                                      | Value           |
| ------------------------------------- | ---------------------------------------------------------------- | --------------- |
| `cron.job.schedule`                   | Interval for how often the job should run.                       | `"*/5 * * * *"` |
| `cron.job.successfulJobsHistoryLimit` | Limit of the successful jobs saved in history as completed pods. | `1`             |
| `cron.pod.restartPolicy`              | The logic for restarting the cron pod.                           | `OnFailure`     |


### Secret parameters

| Name                    | Description                                                                                                                                                                                | Value |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----- |
| `secret.existingSecret` | Name of an existing secret. Check [secret.yaml](https://artifacthub.io/packages/helm/kubitodev/kubernetes-cloudflare-ddns?modal=template&template=secret.yaml) for the required variables. | `""`  |
| `secret.authKey`        | The value of your Cloudflare API Key.                                                                                                                                                      | `""`  |
| `secret.dnsRecord`      | The A record you want to update.                                                                                                                                                           | `""`  |
| `secret.recordId`       | The ID of the record you are updating.                                                                                                                                                     | `""`  |
| `secret.zoneId`         | The zone ID of your domain.                                                                                                                                                                | `""`  |


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

### Getting the API Key

Login to Cloudflare and [create an API key](https://dash.cloudflare.com/profile/api-tokens) for your domain on Cloudflare with the following settings:

```txt
Token Name: `Edit Zone DNS`.
Permissions: `Zone -> DNS -> Edit`.
Zone Resources: `Include -> All zones`.
Client API Filtering can be left as default.
TTL can be left as default.
```

### Getting the Zone ID

Go to the overview tab on Cloudflare for your domain and copy the `Zone ID` key.

### Getting the Record ID

Now you need to get the record ID for the `A` record that you wish to update. To do so, just execute the following command where you will replace the variables with your own:

```bash
export ZONE_ID=<YOUR_ZONE_ID>
export AUTH_KEY=<YOUR_API_KEY>

curl -X GET "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?type=A" \
     -H "X-Auth-Email: your@email.com" \
     -H "Authorization: Bearer $AUTH_KEY" \
     -H "Content-Type: application/json"
```

You will get a JSON object returned, and you can use a prettifier or something to get a better look at it if you wish. When you see the record that you want, copy the `id` field value.

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

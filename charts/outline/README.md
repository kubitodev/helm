# Outline Wiki

The fastest knowledge base for growing teams. Beautiful, realtime collaborative, feature packed, and markdown compatible.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install outline kubitodev/outline
```

## Introduction

This chart helps you create a modern team knowledge base for your internal documentation, product specs, support answers, meeting notes, onboarding, & more.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `outline`:

```console
helm install outline kubitodev/outline
```

The command deploys outline on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `outline` deployment:

```console
helm delete outline
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Global parameters

| Name                       | Description                                                                                                                                      | Value  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| `global.postgresqlEnabled` | Whether to deploy the postgresql subchart. Check the [chart](https://artifacthub.io/packages/helm/bitnami/postgresql) for configuration.         | `true` |
| `global.redisEnabled`      | Whether to deploy the redis chart as subchart. Check [the official chart](https://artifacthub.io/packages/helm/bitnami/redis) for configuration. | `true` |
| `global.minioEnabled`      | Whether to deploy the minio chart as subchart. Check [the official chart](https://artifacthub.io/packages/helm/bitnami/minio) for configuration. | `true` |

### Image parameters

| Name                    | Description                                   | Value                 |
| ----------------------- | --------------------------------------------- | --------------------- |
| `image.repository`      | The Docker repository to pull the image from. | `outlinewiki/outline` |
| `image.tag`             | The image tag to use.                         | `0.78.0-0`            |
| `image.imagePullPolicy` | The logic of image pulling.                   | `IfNotPresent`        |

### Deployment parameters

| Name                                          | Description                                                                                     | Value                     |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------- |
| `replicaCount`                                | The number of replicas to deploy.                                                               | `1`                       |
| `outline.secretKey.value`                     | Direct value for the secret key, if not using Kubernetes secret.                                | `""`                      |
| `outline.secretKey.existingSecret`            | Reference to an existing Kubernetes secret for the secret key (outline-secret-key).             | `""`                      |
| `outline.utilsSecret.value`                   | Direct value for the utilities secret, if not using Kubernetes secret.                          | `""`                      |
| `outline.utilsSecret.existingSecret`          | Reference to an existing Kubernetes secret for the utilities secret (outline-utils-key).        | `""`                      |
| `outline.nodeEnv`                             | The environment in which the application is running (e.g., production, development).            | `production`              |
| `outline.database.connectionPoolMin`          | Minimum number of connections in the database pool.                                             | `""`                      |
| `outline.database.connectionPoolMax`          | Maximum number of connections in the database pool.                                             | `20`                      |
| `outline.database.sslMode`                    | SSL mode for the database connection.                                                           | `disable`                 |
| `outline.service.url`                         | The URL where the application will be accessible.                                               | `https://app.outline.dev` |
| `outline.service.port`                        | The port on which the application will run.                                                     | `3000`                    |
| `outline.fileStorage.type`                    | Type of file storage to be used (e.g., local, s3).                                              | `s3`                      |
| `outline.fileStorage.storageClassName`        | The storageclass name to use                                                                    | `""`                      |
| `outline.fileStorage.storageSize`             | The storageclass size to use                                                                    | `250Gi`                   |
| `outline.fileStorage.localRootDir`            | Local directory path for storing files, if using local storage.                                 | `/var/lib/outline/data`   |
| `outline.fileStorage.uploadMaxSize`           | Maximum file upload size limit.                                                                 | `26214400`                |
| `outline.optional.collaborationUrl`           | URL for collaboration features, if any.                                                         | `""`                      |
| `outline.optional.forceHttps`                 | Forces HTTPS connections for increased security.                                                | `false`                   |
| `outline.optional.enableUpdates`              | Enables automatic application updates.                                                          | `false`                   |
| `outline.optional.webConcurrency`             | Number of concurrent web processes to run.                                                      | `1`                       |
| `outline.optional.fileStorageImportMaxSize`   | Maximum size limit for importing documents.                                                     | `5120000`                 |
| `outline.optional.logLevel`                   | Sets the level of logging detail (e.g., info, error, debug).                                    | `info`                    |
| `outline.optional.googleAnalyticsId`          | Google Analytics ID for website traffic analysis.                                               | `""`                      |
| `outline.optional.iframely.enabled`           | Whether to enable iframely                                                                      | `false`                   |
| `outline.optional.iframely.url`               | URL of the iframely server                                                                      | `"http://iframely:8061"`  |
| `outline.optional.iframely.apiKey`            | api key of the iframely server                                                                  | `""`                      |
| `outline.optional.sentry.dsn`                 | Data Source Name for Sentry, used to report errors.                                             | `""`                      |
| `outline.optional.sentry.tunnel`              | URL for Sentry tunnel, useful for bypassing ad blockers.                                        | `""`                      |
| `outline.optional.smtp.enabled`               | Whether to enable SMTP.                                                                         | `false`                   |
| `outline.optional.smtp.host`                  | Hostname of the SMTP server.                                                                    | `""`                      |
| `outline.optional.smtp.port`                  | Port number for the SMTP server.                                                                | `""`                      |
| `outline.optional.smtp.username`              | Username for SMTP server authentication.                                                        | `""`                      |
| `outline.optional.smtp.existingSecret`        | Reference to an existing Kubernetes secret for SMTP password configuration (smtp-password).     | `""`                      |
| `outline.optional.smtp.password`              | Password for SMTP server authentication.                                                        | `""`                      |
| `outline.optional.smtp.fromEmail`             | Email address used as the sender in outgoing emails.                                            | `hello@example.com`       |
| `outline.optional.smtp.replyEmail`            | Email address used for replies to outgoing emails.                                              | `hello@example.com`       |
| `outline.optional.smtp.tlsCiphers`            | TLS ciphers to use for SMTP connections.                                                        | `""`                      |
| `outline.optional.smtp.secure`                | Boolean indicating whether to use a secure connection for SMTP.                                 | `true`                    |
| `outline.optional.defaultLanguage`            | Default language setting for the application interface.                                         | `en_US`                   |
| `outline.optional.rateLimiter.enabled`        | Enable or disable rate limiting.                                                                | `false`                   |
| `outline.optional.rateLimiter.requests`       | Number of requests allowed per time window.                                                     | `1000`                    |
| `outline.optional.rateLimiter.durationWindow` | Duration of the rate limit window in seconds.                                                   | `60`                      |
| `outline.optional.developmentUnsafeInlineCsp` | Enables unsafe-inline CSP directive in development mode.                                        | `false`                   |
| `outline.optional.ssl.key`                    | Base64 encoded private key for SSL.                                                             | `""`                      |
| `outline.optional.ssl.cert`                   | Base64 encoded certificate for SSL.                                                             | `""`                      |
| `outline.optional.cdnUrl`                     | URL for Content Delivery Network, if used.                                                      | `""`                      |
| `auth.slack.enabled`                          | Whether to enable Slack auth.                                                                   | `false`                   |
| `auth.slack.clientId.value`                   | Direct value for the Slack client ID, if not using Kubernetes secret.                           | `""`                      |
| `auth.slack.clientId.existingSecret`          | Reference to an existing Kubernetes secret for the Slack client ID (slack-client-id).           | `""`                      |
| `auth.slack.clientSecret.value`               | Direct value for the Slack client secret, if not using Kubernetes secret.                       | `""`                      |
| `auth.slack.clientSecret.existingSecret`      | Reference to an existing Kubernetes secret for the Slack client secret (slack-client-secret).   | `""`                      |
| `auth.slack.messageActions`                   | Enables Slack message actions integration.                                                      | `true`                    |
| `auth.slack.appId`                            | Slack application ID.                                                                           | `""`                      |
| `auth.slack.verificationToken`                | Slack verification token.                                                                       | `""`                      |
| `auth.google.enabled`                         | Whether to enable Google auth.                                                                  | `false`                   |
| `auth.google.clientId.value`                  | Direct value for the Google client ID, if not using Kubernetes secret.                          | `""`                      |
| `auth.google.clientId.existingSecret`         | Reference to an existing Kubernetes secret for the Google client ID (google-client-id).         | `""`                      |
| `auth.google.clientSecret.value`              | Direct value for the Google client secret, if not using Kubernetes secret.                      | `""`                      |
| `auth.google.clientSecret.existingSecret`     | Reference to an existing Kubernetes secret for the Google client secret (google-client-secret). | `""`                      |
| `auth.azure.enabled`                          | Whether to enable Azure auth.                                                                   | `false`                   |
| `auth.azure.clientId.value`                   | Direct value for the Azure client ID, if not using Kubernetes secret.                           | `""`                      |
| `auth.azure.clientId.existingSecret`          | Reference to an existing Kubernetes secret for the Azure client ID (azure-client-id).           | `""`                      |
| `auth.azure.clientSecret.value`               | Direct value for the Azure client secret, if not using Kubernetes secret.                       | `""`                      |
| `auth.azure.clientSecret.existingSecret`      | Reference to an existing Kubernetes secret for the Azure client secret (azure-client-secret).   | `""`                      |
| `auth.azure.resourceAppId`                    | Azure resource application ID.                                                                  | `""`                      |
| `auth.oidc.enabled`                           | Whether to enable OIDC auth.                                                                    | `false`                   |
| `auth.oidc.clientId.value`                    | Direct value for the OIDC client ID, if not using Kubernetes secret.                            | `""`                      |
| `auth.oidc.clientId.existingSecret`           | Reference to an existing Kubernetes secret for the OIDC client ID (oidc-client-id).             | `""`                      |
| `auth.oidc.clientSecret.value`                | Direct value for the OIDC client secret, if not using Kubernetes secret.                        | `""`                      |
| `auth.oidc.clientSecret.existingSecret`       | Reference to an existing Kubernetes secret for the OIDC client secret (oidc-client-secret).     | `""`                      |
| `auth.oidc.authUri`                           | OIDC authorization endpoint URI.                                                                | `""`                      |
| `auth.oidc.tokenUri`                          | OIDC token endpoint URI.                                                                        | `""`                      |
| `auth.oidc.userinfoUri`                       | OIDC userinfo endpoint URI.                                                                     | `""`                      |
| `auth.oidc.usernameClaim`                     | OIDC claim used for the username.                                                               | `preferred_username`      |
| `auth.oidc.displayName`                       | Name to display for OIDC authentication.                                                        | `OpenID Connect`          |
| `auth.oidc.scopes`                            | OIDC scopes to request.                                                                         | `openid profile email`    |

### Ingress parameters

| Name                                                    | Description                                           | Value    |
| ------------------------------------------------------- | ----------------------------------------------------- | -------- |
| `ingress.enabled`                                       | Whether to enable the ingress.                        | `false`  |
| `ingress.annotations`                                   | Annotations for the Ingress resource.                 | `{}`     |
| `ingress.tls.enabled`                                   | Enable TLS for the Ingress.                           | `false`  |
| `ingress.tls.hosts`                                     | List of hosts for which the TLS certificate is valid. | `[]`     |
| `ingress.rules.host`                                    | The host name that the Ingress will respond to.       | `""`     |
| `ingress.rules.http[0].paths.path`                      | URL path for the HTTP rule.                           | `/`      |
| `ingress.rules.http[0].paths.pathType`                  | Type of path matching.                                | `Prefix` |
| `ingress.rules.http[0].paths.backend.service.name`      | Name of the service that serves the specified path.   | `""`     |
| `ingress.rules.http[0].paths.backend.service.port.name` | Name of the service port.                             | `web`    |

### Redis parameters

| Name                 | Description                                                   | Value        |
| -------------------- | ------------------------------------------------------------- | ------------ |
| `redis.architecture` | Redis deployment architecture: 'standalone' or 'replication'. | `standalone` |
| `redis.auth.enabled` | Enable or disable authentication for Redis.                   | `false`      |

### PostgreSQL parameters

| Name                               | Description                                                                                                                                         | Value        |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| `postgresql.architecture`          | PostgreSQL deployment architecture: 'standalone' or 'replication'.                                                                                  | `standalone` |
| `postgresql.auth.host`             | the hostname of the database default is {{ .Release.Name }}-postgresql .                                                                            | `""`         |
| `postgresql.auth.database`         | Name of the default database created on initial startup.                                                                                            | `outline`    |
| `postgresql.auth.username`         | Username for the default database.                                                                                                                  | `outline`    |
| `postgresql.auth.existingSecret`   | Reference to an existing Kubernetes secret containing the database credentials. Use 'password' for user, and 'postgres-password' for postgres user. | `""`         |
| `postgresql.auth.password`         | Password for the default database user (used if existingSecret is not specified).                                                                   | `""`         |
| `postgresql.auth.postgresPassword` | Password for the 'postgres' administrative user (used if existingSecret is not specified).                                                          | `""`         |

### Minio parameters

| Name                              | Description                                                                                                                              | Value                                       |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `minio.mode`                      | MinIO deployment mode: 'standalone' or 'distributed'.                                                                                    | `standalone`                                |
| `minio.disableWebUI`              | Enable or disable the MinIO web user interface.                                                                                          | `false`                                     |
| `minio.defaultBuckets`            | Comma-separated list of default buckets to create on startup.                                                                            | `outline`                                   |
| `minio.auth.rootUser`             | Root user name for MinIO access.                                                                                                         | `""`                                        |
| `minio.auth.existingSecret`       | Reference to an existing Kubernetes secret containing MinIO credentials. Use 'root-user' for user, and 'root-password' for the password. | `""`                                        |
| `minio.auth.rootPassword`         | Root password for MinIO access (used if existingSecret is not specified).                                                                | `""`                                        |
| `minio.ingress.enabled`           | Enable or disable Ingress for MinIO.                                                                                                     | `false`                                     |
| `minio.ingress.hostname`          | Default host for the ingress resource for MinIO.                                                                                         | `outline.server.example`                    |
| `minio.s3Config.region`           | Direct value for the AWS region, if not using Kubernetes secret.                                                                         | `eu-central-1`                              |
| `minio.s3Config.accelerateUrl`    | URL for S3 Transfer Acceleration.                                                                                                        | `""`                                        |
| `minio.s3Config.uploadMaxSize`    | Max size for S3 uploads.                                                                                                                 | `26214400`                                  |
| `minio.s3Config.uploadBucketUrl`  | URL to the S3 bucket for uploads.                                                                                                        | `http://minio.minio.svc.cluster.local:4569` |
| `minio.s3Config.uploadBucketName` | Name of the S3 bucket for uploads.                                                                                                       | `outline`                                   |
| `minio.s3Config.forcePathStyle`   | Whether to force path style URLs for S3.                                                                                                 | `true`                                      |
| `minio.s3Config.acl`              | Default ACL for S3 objects.                                                                                                              | `private`                                   |

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

### Basic installation with SMTP, S3, Keycloak OIDC, and existing PostgreSQL database

This example will show how to set up a basic installation of Outline using SMTP and S3 (Minio) as file storage, as well as an existing PostgreSQL database. To use a newly created PostgreSQL database, enable the sub chart. It also shows you how a Keycloak OIDC setup works:

```yaml
global:
  postgresqlEnabled: false
  minioEnabled: true

outline:
  secretKey:
    existingSecret: outline-secret # see values for key naming
  utilsSecret:
    existingSecret: outline-secret # see values for key naming
  service:
    url: https://docs.example.com
    port: 3000
  optional:
    forceHttps: "true"
    logLevel: debug
    smtp:
      enabled: "true"
      host: smtp.gmail.com
      port: "465"
      username: example
      existingSecret: outline-secret # see values for key naming
      fromEmail: "smtp@example.com"
      replyEmail: noreply@example.com
      secure: true
  fileStorage:
    type: s3

postgresql:
  auth:
    database: outline
    username: kubito
    existingSecret: outline-secret # see values for key naming

auth:
  oidc:
    enabled: true
    clientId:
      existingSecret: outline-secret
    clientSecret:
      existingSecret: outline-secret
    authUri: "https://auth.example.com/realms/outline/protocol/openid-connect/auth"
    tokenUri: "https://auth.example.com/realms/outline/protocol/openid-connect/token"
    userinfoUri: "https://auth.example.com/realms/outline/protocol/openid-connect/userinfo"
    displayName: Keycloak
    usernameClaim: username
    scopes: openid profile email

minio:
  auth:
    existingSecret: outline-secret
  s3Config:
    region: eu-central-1
    uploadMaxSize: "26214400"
    uploadBucketUrl: https://s3.example.com
    uploadBucketName: outline
    forcePathStyle: true
    acl: private
```

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

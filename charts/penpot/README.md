# Penpot

Penpot is the first Open Source design and prototyping platform meant for cross-domain teams. Non dependent on operating systems, Penpot is web based and works with open standards (SVG). Penpot invites designers all over the world to fall in love with open source while getting developers excited about the design process in return.

## TL;DR

```console
helm repo add kubitodev https://charts.kubito.dev
helm install penpot kubitodev/penpot
```

## Introduction

Penpot makes design and prototyping accessible to every team in the world. It has a clear focus on design and code teams and its capabilities reflect exactly that. The less hand-off mindset, the more fun for everyone. Being web based, Penpot is not dependent on operating systems or local installations, you will only need to run a modern browser. Using SVG as no other design and prototyping tool does, Penpot files sport compatibility with most of the vectorial tools, are tech friendly and extremely easy to use on the web. It makes sure you will always own your work.

## Prerequisites

- Kubernetes 1.18+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `penpot`:

```console
helm install penpot kubitodev/penpot
```

The command deploys penpot on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `penpot` deployment:

```console
helm delete penpot
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Global parameters

| Name                       | Description                                                                                                                                                        | Value  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| `global.postgresqlEnabled` | Whether to deploy the Bitnami PostgreSQL chart as subchart. Check [the official chart](https://artifacthub.io/packages/helm/bitnami/postgresql) for configuration. | `true` |
| `global.redisEnabled`      | Whether to deploy the Bitnami Redis chart as subchart. Check [the official chart](https://artifacthub.io/packages/helm/bitnami/redis) for configuration.           | `true` |
| `global.imagePullSecrets`  | Global Docker registry secret names as an array.                                                                                                                   | `[]`   |

### Common parameters

| Name                         | Description                                                                                                             | Value  |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------ |
| `serviceAccount.enabled`     | Specifies whether a ServiceAccount should be created.                                                                   | `true` |
| `serviceAccount.annotations` | Annotations for service account. Evaluated as a template. Only used if `create` is `true`.                              | `{}`   |
| `serviceAccount.name`        | The name of the ServiceAccount to use. If not set and enabled is true, a name is generated using the fullname template. | `""`   |

### Backend parameters

| Name                                                        | Description                                                                                                                                                     | Value               |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `backend.image.repository`                                  | The Docker repository to pull the image from.                                                                                                                   | `penpotapp/backend` |
| `backend.image.tag`                                         | The image tag to use.                                                                                                                                           | `2.2.1`             |
| `backend.image.imagePullPolicy`                             | The image pull policy to use.                                                                                                                                   | `IfNotPresent`      |
| `backend.replicaCount`                                      | The number of replicas to deploy.                                                                                                                               | `1`                 |
| `backend.service.type`                                      | The service type to create.                                                                                                                                     | `ClusterIP`         |
| `backend.service.port`                                      | The service port to use.                                                                                                                                        | `6060`              |
| `backend.podSecurityContext.enabled`                        | Enabled Penpot pods' security context. Ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod       | `true`              |
| `backend.podSecurityContext.fsGroup`                        | Set Penpot pod's security context fsGroup                                                                                                                       | `1001`              |
| `backend.containerSecurityContext.enabled`                  | Enabled Penpot containers' security context. Ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod | `true`              |
| `backend.containerSecurityContext.runAsUser`                | Set Penpot containers' security context runAsUser                                                                                                               | `1001`              |
| `backend.containerSecurityContext.allowPrivilegeEscalation` | Set Penpot containers' security context allowPrivilegeEscalation                                                                                                | `false`             |
| `backend.containerSecurityContext.capabilities.drop`        | Set Penpot containers' security context capabilities to be dropped                                                                                              | `["all"]`           |
| `backend.containerSecurityContext.readOnlyRootFilesystem`   | Set Penpot containers' security context readOnlyRootFilesystem                                                                                                  | `false`             |
| `backend.containerSecurityContext.runAsNonRoot`             | Set Penpot container's security context runAsNonRoot                                                                                                            | `true`              |
| `backend.affinity`                                          | Affinity for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity                         | `{}`                |
| `backend.nodeSelector`                                      | Node labels for Penpot pods assignment. Ref: https://kubernetes.io/docs/user-guide/node-selection/                                                              | `{}`                |
| `backend.tolerations`                                       | Tolerations for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/                                            | `[]`                |
| `backend.resources.limits`                                  | The resources limits for the Penpot backend containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/                                           | `{}`                |
| `backend.resources.requests`                                | The requested resources for the Penpot backend containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/                                        | `{}`                |

### Frontend parameters

| Name                             | Description                                                                                                                             | Value                |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| `frontend.image.repository`      | The Docker repository to pull the image from.                                                                                           | `penpotapp/frontend` |
| `frontend.image.tag`             | The image tag to use.                                                                                                                   | `2.2.1`              |
| `frontend.image.imagePullPolicy` | The image pull policy to use.                                                                                                           | `IfNotPresent`       |
| `frontend.replicaCount`          | The number of replicas to deploy.                                                                                                       | `1`                  |
| `frontend.service.type`          | The service type to create.                                                                                                             | `ClusterIP`          |
| `frontend.service.port`          | The service port to use. Don't change unless you plan to configure NGINX yourself.                                                      | `8080`               |
| `frontend.ingress.enabled`       | Enable ingress record generation for Penpot frontend.                                                                                   | `false`              |
| `frontend.ingress.annotations`   | Mapped annotations for the frontend ingress.                                                                                            | `{}`                 |
| `frontend.ingress.hosts`         | Array style hosts for the frontend ingress.                                                                                             | `[]`                 |
| `frontend.ingress.tls`           | Array style TLS secrets for the frontend ingress.                                                                                       | `[]`                 |
| `frontend.affinity`              | Affinity for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity | `{}`                 |
| `frontend.nodeSelector`          | Node labels for Penpot pods assignment. Ref: https://kubernetes.io/docs/user-guide/node-selection/                                      | `{}`                 |
| `frontend.tolerations`           | Tolerations for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/                    | `[]`                 |
| `frontend.resources.limits`      | The resources limits for the Penpot frontend containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/                  | `{}`                 |
| `frontend.resources.requests`    | The requested resources for the Penpot frontend containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/               | `{}`                 |

### Exporter parameters

| Name                                                         | Description                                                                                                                                                     | Value                |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| `exporter.image.repository`                                  | The Docker repository to pull the image from.                                                                                                                   | `penpotapp/exporter` |
| `exporter.image.tag`                                         | The image tag to use.                                                                                                                                           | `2.2.1`              |
| `exporter.image.imagePullPolicy`                             | The image pull policy to use.                                                                                                                                   | `IfNotPresent`       |
| `exporter.replicaCount`                                      | The number of replicas to deploy.                                                                                                                               | `1`                  |
| `exporter.service.type`                                      | The service type to create.                                                                                                                                     | `ClusterIP`          |
| `exporter.service.port`                                      | The service port to use.                                                                                                                                        | `6061`               |
| `exporter.podSecurityContext.enabled`                        | Enabled Penpot pods' security context. Ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod       | `true`               |
| `exporter.podSecurityContext.fsGroup`                        | Set Penpot pod's security context fsGroup                                                                                                                       | `1001`               |
| `exporter.containerSecurityContext.enabled`                  | Enabled Penpot containers' security context. Ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod | `true`               |
| `exporter.containerSecurityContext.runAsUser`                | Set Penpot containers' security context runAsUser                                                                                                               | `1001`               |
| `exporter.containerSecurityContext.allowPrivilegeEscalation` | Set Penpot containers' security context allowPrivilegeEscalation                                                                                                | `false`              |
| `exporter.containerSecurityContext.capabilities.drop`        | Set Penpot containers' security context capabilities to be dropped                                                                                              | `["all"]`            |
| `exporter.containerSecurityContext.readOnlyRootFilesystem`   | Set Penpot containers' security context readOnlyRootFilesystem                                                                                                  | `false`              |
| `exporter.containerSecurityContext.runAsNonRoot`             | Set Penpot container's security context runAsNonRoot                                                                                                            | `true`               |
| `exporter.affinity`                                          | Affinity for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity                         | `{}`                 |
| `exporter.nodeSelector`                                      | Node labels for Penpot pods assignment. Ref: https://kubernetes.io/docs/user-guide/node-selection/                                                              | `{}`                 |
| `exporter.tolerations`                                       | Tolerations for Penpot pods assignment. Ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/                                            | `[]`                 |
| `exporter.resources.limits`                                  | The resources limits for the Penpot exporter containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/                                          | `{}`                 |
| `exporter.resources.requests`                                | The requested resources for the Penpot exporter containers. Ref: https://kubernetes.io/docs/user-guide/compute-resources/                                       | `{}`                 |

### Persistence parameters

| Name                        | Description                                                                                                                                    | Value               |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `persistence.enabled`       | Enable persistence using Persistent Volume Claims.                                                                                             | `false`             |
| `persistence.storageClass`  | Persistent Volume storage class. If undefined (the default) or set to null, no storageClassName spec is set, choosing the default provisioner. | `""`                |
| `persistence.size`          | Persistent Volume size.                                                                                                                        | `8Gi`               |
| `persistence.existingClaim` | The name of an existing PVC to use for persistence.                                                                                            | `""`                |
| `persistence.accessModes`   | Persistent Volume access modes.                                                                                                                | `["ReadWriteOnce"]` |
| `persistence.annotations`   | Persistent Volume Claim annotations.                                                                                                           | `{}`                |

### Penpot Configuration parameters

| Name                                                | Description                                                                                                                                                                                                                         | Value                                                                      |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `config.publicURI`                                  | The public domain to serve Penpot on. Set `disable-secure-session-cookies` in the flags if you plan on serving it on a non HTTPS domain.                                                                                            | `http://localhost:8080`                                                    |
| `config.flags`                                      | The feature flags to enable. Check [the official docs](https://help.penpot.app/technical-guide/configuration/) for more info.                                                                                                       | `enable-registration enable-login disable-demo-users disable-demo-warning` |
| `config.internalResolver`                           | The internal cluster resolver to use. Required for the frontend NGINX configuration to fetch fonts. Only replace if you know what you're doing.                                                                                     | `kube-dns.kube-system.svc.cluster.local`                                   |
| `config.apiSecretKey`                               | A random secret key needed for persistent user sessions. Generate with `openssl rand -hex 16` for example.                                                                                                                          | `b46a12cb4bedc6b9df8cb3f18c708b65`                                         |
| `config.postgresql.host`                            | The PostgreSQL host to connect to.                                                                                                                                                                                                  | `postgresql.penpot.svc.cluster.local`                                      |
| `config.postgresql.port`                            | The PostgreSQL host port to use.                                                                                                                                                                                                    | `5432`                                                                     |
| `config.postgresql.database`                        | The PostgreSQL database to use.                                                                                                                                                                                                     | `""`                                                                       |
| `config.postgresql.username`                        | The database username to use.                                                                                                                                                                                                       | `""`                                                                       |
| `config.postgresql.password`                        | The database username to use.                                                                                                                                                                                                       | `""`                                                                       |
| `config.postgresql.existingSecret`                  | The name of an existing secret.                                                                                                                                                                                                     | `""`                                                                       |
| `config.postgresql.secretKeys.usernameKey`          | The username key to use from an existing secret.                                                                                                                                                                                    | `""`                                                                       |
| `config.postgresql.secretKeys.passwordKey`          | The password key to use from an existing secret.                                                                                                                                                                                    | `""`                                                                       |
| `config.redis.host`                                 | The Redis host to connect to.                                                                                                                                                                                                       | `penpot-redis-headless.penpot.svc.cluster.local`                           |
| `config.redis.port`                                 | The Redis host port to use.                                                                                                                                                                                                         | `6379`                                                                     |
| `config.redis.database`                             | The Redis database to connect to.                                                                                                                                                                                                   | `0`                                                                        |
| `config.assets.storageBackend`                      | The storage backend for assets to use. Use `assets-fs` for filesystem, and `assets-s3` for S3.                                                                                                                                      | `assets-fs`                                                                |
| `config.assets.filesystem.directory`                | The storage directory to use if you chose the filesystem storage backend.                                                                                                                                                           | `/opt/data/assets`                                                         |
| `config.assets.s3.accessKeyID`                      | The S3 access key ID to use if you chose the S3 storage backend.                                                                                                                                                                    | `""`                                                                       |
| `config.assets.s3.secretAccessKey`                  | The S3 secret access key to use if you chose the S3 storage backend.                                                                                                                                                                | `""`                                                                       |
| `config.assets.s3.region`                           | The S3 region to use if you chose the S3 storage backend.                                                                                                                                                                           | `""`                                                                       |
| `config.assets.s3.bucket`                           | The name of the S3 bucket to use if you chose the S3 storage backend.                                                                                                                                                               | `""`                                                                       |
| `config.assets.s3.endpointURI`                      | The S3 endpoint URI to use if you chose the S3 storage backend.                                                                                                                                                                     | `""`                                                                       |
| `config.assets.s3.existingSecret`                   | The name of an existing secret.                                                                                                                                                                                                     | `""`                                                                       |
| `config.assets.s3.secretKeys.accessKeyIDKey`        | The S3 access key ID to use from an existing secret.                                                                                                                                                                                | `""`                                                                       |
| `config.assets.s3.secretKeys.secretAccessKey`       | The S3 secret access key to use from an existing secret.                                                                                                                                                                            | `""`                                                                       |
| `config.assets.s3.secretKeys.endpointURIKey`        | The S3 endpoint URI to use from an existing secret.                                                                                                                                                                                 | `""`                                                                       |
| `config.telemetryEnabled`                           | Whether to enable sending of anonymous telemetry data.                                                                                                                                                                              | `false`                                                                    |
| `config.smtp.enabled`                               | Whether to enable SMTP configuration. You also need to add the 'enable-smtp' flag to the PENPOT_FLAGS variable.                                                                                                                     | `false`                                                                    |
| `config.smtp.defaultFrom`                           | The SMTP default email to send from.                                                                                                                                                                                                | `""`                                                                       |
| `config.smtp.defaultReplyTo`                        | The SMTP default email to reply to.                                                                                                                                                                                                 | `""`                                                                       |
| `config.smtp.host`                                  | The SMTP host to use.                                                                                                                                                                                                               | `""`                                                                       |
| `config.smtp.port`                                  | The SMTP host port to use.                                                                                                                                                                                                          | `""`                                                                       |
| `config.smtp.username`                              | The SMTP username to use.                                                                                                                                                                                                           | `""`                                                                       |
| `config.smtp.password`                              | The SMTP password to use.                                                                                                                                                                                                           | `""`                                                                       |
| `config.smtp.tls`                                   | Whether to use TLS for the SMTP connection.                                                                                                                                                                                         | `true`                                                                     |
| `config.smtp.ssl`                                   | Whether to use SSL for the SMTP connection.                                                                                                                                                                                         | `false`                                                                    |
| `config.smtp.existingSecret`                        | The name of an existing secret.                                                                                                                                                                                                     | `""`                                                                       |
| `config.smtp.secretKeys.usernameKey`                | The SMTP username to use from an existing secret.                                                                                                                                                                                   | `""`                                                                       |
| `config.smtp.secretKeys.passwordKey`                | The SMTP password to use from an existing secret.                                                                                                                                                                                   | `""`                                                                       |
| `config.registrationDomainWhitelist`                | Comma separated list of allowed domains to register. Empty to allow all domains.                                                                                                                                                    | `""`                                                                       |
| `config.providers.google.enabled`                   | Whether to enable Google configuration. To enable Google auth, add `enable-login-with-google` to the flags.                                                                                                                         | `false`                                                                    |
| `config.providers.google.clientID`                  | The Google client ID to use. To enable Google auth, add `enable-login-with-google` to the flags.                                                                                                                                    | `""`                                                                       |
| `config.providers.google.clientSecret`              | The Google client secret to use. To enable Google auth, add `enable-login-with-google` to the flags.                                                                                                                                | `""`                                                                       |
| `config.providers.github.enabled`                   | Whether to enable GitHub configuration. To enable GitHub auth, also add `enable-login-with-github` to the flags.                                                                                                                    | `false`                                                                    |
| `config.providers.github.clientID`                  | The GitHub client ID to use.                                                                                                                                                                                                        | `""`                                                                       |
| `config.providers.github.clientSecret`              | The GitHub client secret to use.                                                                                                                                                                                                    | `""`                                                                       |
| `config.providers.gitlab.enabled`                   | Whether to enable GitLab configuration. To enable GitLab auth, also add `enable-login-with-gitlab` to the flags.                                                                                                                    | `false`                                                                    |
| `config.providers.gitlab.baseURI`                   | The GitLab base URI to use.                                                                                                                                                                                                         | `https://gitlab.com`                                                       |
| `config.providers.gitlab.clientID`                  | The GitLab client ID to use.                                                                                                                                                                                                        | `""`                                                                       |
| `config.providers.gitlab.clientSecret`              | The GitLab client secret to use.                                                                                                                                                                                                    | `""`                                                                       |
| `config.providers.oidc.enabled`                     | Whether to enable OIDC configuration. To enable OpenID Connect auth, also add `enable-login-with-oidc` to the flags.                                                                                                                | `false`                                                                    |
| `config.providers.oidc.baseURI`                     | The OpenID Connect base URI to use.                                                                                                                                                                                                 | `""`                                                                       |
| `config.providers.oidc.clientID`                    | The OpenID Connect client ID to use.                                                                                                                                                                                                | `""`                                                                       |
| `config.providers.oidc.clientSecret`                | The OpenID Connect client secret to use.                                                                                                                                                                                            | `""`                                                                       |
| `config.providers.oidc.authURI`                     | Optional OpenID Connect auth URI to use. Auto discovered if not provided.                                                                                                                                                           | `""`                                                                       |
| `config.providers.oidc.tokenURI`                    | Optional OpenID Connect token URI to use. Auto discovered if not provided.                                                                                                                                                          | `""`                                                                       |
| `config.providers.oidc.userURI`                     | Optional OpenID Connect user URI to use. Auto discovered if not provided.                                                                                                                                                           | `""`                                                                       |
| `config.providers.oidc.roles`                       | Optional OpenID Connect roles to use. If no role is provided, roles checking  disabled.                                                                                                                                             | `""`                                                                       |
| `config.providers.oidc.rolesAttribute`              | Optional OpenID Connect roles attribute to use. If not provided, the roles checking will be disabled.                                                                                                                               | `""`                                                                       |
| `config.providers.oidc.scopes`                      | Optional OpenID Connect scopes to use. This settings allow overwrite the required scopes, use with caution because penpot requres at least `name` and `email` attrs found on the user info. Optional, defaults to `openid profile`. | `""`                                                                       |
| `config.providers.oidc.nameAttribute`               | Optional OpenID Connect name attribute to use. If not provided, the `name` prop will be used.                                                                                                                                       | `""`                                                                       |
| `config.providers.oidc.emailAttribute`              | Optional OpenID Connect email attribute to use. If not provided, the `email` prop will be used.                                                                                                                                     | `""`                                                                       |
| `config.providers.ldap.enabled`                     | Whether to enable LDAP configuration. To enable LDAP, also add `enable-login-with-ldap` to the flags.                                                                                                                               | `false`                                                                    |
| `config.providers.ldap.host`                        | The LDAP host to use.                                                                                                                                                                                                               | `ldap`                                                                     |
| `config.providers.ldap.port`                        | The LDAP port to use.                                                                                                                                                                                                               | `10389`                                                                    |
| `config.providers.ldap.ssl`                         | Whether to use SSL for the LDAP connection.                                                                                                                                                                                         | `false`                                                                    |
| `config.providers.ldap.startTLS`                    | Whether to utilize StartTLS for the LDAP connection.                                                                                                                                                                                | `false`                                                                    |
| `config.providers.ldap.baseDN`                      | The LDAP base DN to use.                                                                                                                                                                                                            | `ou=people,dc=planetexpress,dc=com`                                        |
| `config.providers.ldap.bindDN`                      | The LDAP bind DN to use.                                                                                                                                                                                                            | `cn=admin,dc=planetexpress,dc=com`                                         |
| `config.providers.ldap.bindPassword`                | The LDAP bind password to use.                                                                                                                                                                                                      | `GoodNewsEveryone`                                                         |
| `config.providers.ldap.attributesUsername`          | The LDAP attributes username to use.                                                                                                                                                                                                | `uid`                                                                      |
| `config.providers.ldap.attributesEmail`             | The LDAP attributes email to use.                                                                                                                                                                                                   | `mail`                                                                     |
| `config.providers.ldap.attributesFullname`          | The LDAP attributes fullname to use.                                                                                                                                                                                                | `cn`                                                                       |
| `config.providers.ldap.attributesPhoto`             | The LDAP attributes photo format to use.                                                                                                                                                                                            | `jpegPhoto`                                                                |
| `config.providers.existingSecret`                   | The name of an existing secret to use.                                                                                                                                                                                              | `""`                                                                       |
| `config.providers.secretKeys.googleClientIDKey`     | The Google client ID key to use from an existing secret.                                                                                                                                                                            | `""`                                                                       |
| `config.providers.secretKeys.googleClientSecretKey` | The Google client secret key to use from an existing secret.                                                                                                                                                                        | `""`                                                                       |
| `config.providers.secretKeys.githubClientIDKey`     | The GitHub client ID key to use from an existing secret.                                                                                                                                                                            | `""`                                                                       |
| `config.providers.secretKeys.githubClientSecretKey` | The GitHub client secret key to use from an existing secret.                                                                                                                                                                        | `""`                                                                       |
| `config.providers.secretKeys.gitlabClientIDKey`     | The GitLab client ID key to use from an existing secret.                                                                                                                                                                            | `""`                                                                       |
| `config.providers.secretKeys.gitlabClientSecretKey` | The GitLab client secret key to use from an existing secret.                                                                                                                                                                        | `""`                                                                       |
| `config.providers.secretKeys.oidcClientIDKey`       | The OpenID Connect client ID key to use from an existing secret.                                                                                                                                                                    | `""`                                                                       |
| `config.providers.secretKeys.oidcClientSecretKey`   | The OpenID Connect client secret key to use from an existing secret.                                                                                                                                                                | `""`                                                                       |

### PostgreSQL configuration (Check for [more parameters here](https://artifacthub.io/packages/helm/bitnami/postgresql))

| Name                       | Description                             | Value            |
| -------------------------- | --------------------------------------- | ---------------- |
| `postgresql.auth.username` | Name for a custom user to create.       | `example`        |
| `postgresql.auth.password` | Password for the custom user to create. | `secretpassword` |
| `postgresql.auth.database` | Name for a custom database to create.   | `penpot`         |

### Redis configuration (Check for [more parameters here](https://artifacthub.io/packages/helm/bitnami/redis))

| Name                 | Description                                | Value   |
| -------------------- | ------------------------------------------ | ------- |
| `redis.auth.enabled` | Whether to enable password authentication. | `false` |


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

## Common configuration

There are two types of configuration: options (properties that requieres some value) and flags (that just enables or disables something). The PENPOT_FLAGS environment variable will have an ordered list of strings using this format: `<enable|disable>-<flag-name>`.

Regarding the flags, they are all listed in the [official docs](https://help.penpot.app/technical-guide/configuration), and here are the [additional flags](https://help.penpot.app/technical-guide/configuration/#other-flags) which are not mentioned in the chart configuration above, but you can still use them!

## Authentication providers

For configuration of the authentication with third-party auth providers you will need to configure penpot and set the correct callback of your penpot instance in the auth-provider configuration. The callback has the following format:

```txt
<https://<your_domain>/api/auth/oauth/<oauth_provider>/callback>
```

You will need to change `<your_domain>` and `<oauth_provider>` according to your setup. This is how it looks with the `gitlab.com` provider:

```txt
<https://<your_domain>/api/auth/oauth/gitlab/callback>
```

## Redis configuration

The redis configuration is very simple, just provide a valid Redis URI. Redis is used mainly for websocket notifications coordination. Currently just a non authentication connection is supported. Make sure to set persistence related settings under the values if using the dependent chart.

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

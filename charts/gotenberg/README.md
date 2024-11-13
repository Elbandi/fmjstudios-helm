# FMJ Studios - Gotenberg Helm Chart <img src="https://user-images.githubusercontent.com/8983173/130322857-185831e2-f041-46eb-a17f-0a69d066c4e5.png" alt="Gotenberg Logo" width="175" height="175" align="right" loading="lazy"/>

Gotenberg is an open-source, Docker-powered stateless API for PDF files. It provides a developer friendly API to
interact with powerful tools like Chromium and LibreOffice for converting numerous document formats (HTML, Markdown,
Word, Excel, etc.) into PDF files and more. It delivers all of these features within a single Docker image available
on [Docker Hub](https://hub.docker.com/r/gotenberg/gotenberg).

> Head to the [Gotenberg Homepage](https://gotenberg.dev/) for
> in-depth [documentation](https://gotenberg.dev/docs/getting-started/introduction)
> and [configuration guides](https://gotenberg.dev/docs/configuration).

## ✨ TL;DR

### Helm Repository Installation

```shell
helm repo add fmjstudios https://fmjstudios.github.io/helm
helm install gotenberg fmjstudios/gotenberg --version X.Y.Z
```

### OCI Installation

```shell
helm install oci://ghcr.io/fmjstudios/helm/gotenberg:X.Y.Z
```

## Introduction

This chart bootstraps a Gotenberg [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) on
a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh/) package manager. For cluster networking
a [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
and [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) manifest is also created, whereas the
Ingress needs to be explicitly enabled. Lastly the chart configures
a [PodDisruptionBudget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)
and [HorizontalPodAutoscaler](https://kubernetes.io/de/docs/tasks/run-application/horizontal-pod-autoscale/) if
enabled. [RBAC manifests](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) are enabled by default.

The chart supports the configuration of all [Gotenberg CLI options](https://gotenberg.dev/docs/configuration) via
the `gotenberg` key in Helm's _values_ and makes use of the official Docker Hub container image, although this is
configurable via the Image Parameters.

## Parameters

### Image parameters

| Name                | Description                                                         | Value                 |
| ------------------- | ------------------------------------------------------------------- | --------------------- |
| `image.registry`    | The Docker registry to pull the image from                          | `docker.io`           |
| `image.repository`  | The registry repository to pull the image from                      | `gotenberg/gotenberg` |
| `image.tag`         | The image tag to pull                                               | `8.7.0`               |
| `image.digest`      | The image digest to pull                                            | `""`                  |
| `image.pullPolicy`  | The Kubernetes image pull policy                                    | `IfNotPresent`        |
| `image.pullSecrets` | A list of secrets to use for pulling images from private registries | `[]`                  |

### Name overrides

| Name               | Description                                     | Value |
| ------------------ | ----------------------------------------------- | ----- |
| `nameOverride`     | String to partially override gotenberg.fullname | `""`  |
| `fullnameOverride` | String to fully override gotenberg.fullname     | `""`  |

### Gotenberg Configuration parameters

| Name                                          | Description                                                                      | Value       |
| --------------------------------------------- | -------------------------------------------------------------------------------- | ----------- |
| `gotenberg.api.port`                          | The port the API should listen on                                                | `3000`      |
| `gotenberg.api.tlsCertFile`                   | Disable health check logging                                                     | `""`        |
| `gotenberg.api.tlsKeyFile`                    | Disable health check logging                                                     | `""`        |
| `gotenberg.api.startTimeout`                  | Set the time limit for the API to start                                          | `30s`       |
| `gotenberg.api.timeout`                       | Set the time limit for requests                                                  | `30s`       |
| `gotenberg.api.rootPath`                      | Set the root path of the API                                                     | `"/"`       |
| `gotenberg.api.traceHeader`                   | Set the header name to use for identifying requests                              | `""`        |
| `gotenberg.api.disableHealthCheckLogging`     | Disable health check logging                                                     | `false`     |
| `gotenberg.basicAuth.enabled`                 | Enable basic authentication                                                      | `false`     |
| `gotenberg.basicAuth.username`                | Set a username for HTTP Basic Auth                                               | `""`        |
| `gotenberg.basicAuth.password`                | Set a password for HTTP Basic Auth                                               | `""`        |
| `gotenberg.basicAuth.existingSecret`          | The name of an existing `basic-auth` secret                                      | `""`        |
| `gotenberg.chromium.restartAfter`             | Number of conversions after which Chromium will auto-restart                     | `0`         |
| `gotenberg.chromium.maxQueueSize`             | Maximum request queue size for Chromium                                          | `0`         |
| `gotenberg.chromium.autoStart`                | Automatically launch Chromium upon initialization if set to true                 | `false`     |
| `gotenberg.chromium.startTimeout`             | Maximum duration to wait for Chromium to start or restart                        | `20s`       |
| `gotenberg.chromium.allowFileAccessFromFiles` | Allow file:// URIs to read other file:// URIs                                    | `false`     |
| `gotenberg.chromium.allowInsecureLocalhost`   | Ignore TLS/SSL errors on localhost                                               | `false`     |
| `gotenberg.chromium.allowList`                | Set the allowed URLs for Chromium using a regular expression - defaults to 'All' | `""`        |
| `gotenberg.chromium.denyList`                 | Set the denied URLs for Chromium using a regular expression                      | `""`        |
| `gotenberg.chromium.ignoreCertificateErrors`  | Ignore the certificate errors                                                    | `false`     |
| `gotenberg.chromium.disableWebSecurity`       | Don't enforce the same-origin policy                                             | `false`     |
| `gotenberg.chromium.incognito`                | Start Chromium with incognito mode                                               | `false`     |
| `gotenberg.chromium.hostResolverRules`        | Set custom mappings to the host resolver                                         | `""`        |
| `gotenberg.chromium.proxyServer`              | Set the outbound proxy server; this switch only affects HTTP and HTTPS requests  | `""`        |
| `gotenberg.chromium.clearCache`               | Clear Chromium cache between each conversion                                     | `false`     |
| `gotenberg.chromium.clearCookies`             | Clear Chromium cookies between each conversion                                   | `false`     |
| `gotenberg.chromium.disableJavaScript`        | Disable JavaScript                                                               | `false`     |
| `gotenberg.chromium.disableRoutes`            | Disable the routes                                                               | `false`     |
| `gotenberg.libreOffice.restartAfter`          | Number of conversions after which LibreOffice will automatically restart         | `10`        |
| `gotenberg.libreOffice.maxQueueSize`          | Maximum request queue size for LibreOffice                                       | `0`         |
| `gotenberg.libreOffice.autoStart`             | Automatically launch LibreOffice upon initialization if set to true              | `false`     |
| `gotenberg.libreOffice.startTimeout`          | Maximum duration to wait for LibreOffice to start or restart                     | `20s`       |
| `gotenberg.libreOffice.disableRoutes`         | Disable the route                                                                | `false`     |
| `gotenberg.pdf.engines`                       | Set the PDF engines and their order - defaults to 'All'                          | `""`        |
| `gotenberg.pdf.disableRoutes`                 | Disable the routes                                                               | `false`     |
| `gotenberg.webhook.allowList`                 | Set the allowed URLs for the webhook feature using a regular expression          | `""`        |
| `gotenberg.webhook.denyList`                  | Set the denied URLs for the webhook feature using a regular expression           | `""`        |
| `gotenberg.webhook.errorAllowList`            | Set the allowed URLs in case of an error using a regular expression              | `""`        |
| `gotenberg.webhook.errorDenyList`             | Set the denied URLs in case of an error using a regular expression               | `""`        |
| `gotenberg.webhook.maxRetry`                  | Set the maximum number of retries                                                | `4`         |
| `gotenberg.webhook.retryMinWait`              | Set the minimum duration to wait before trying to call the webhook again         | `1s`        |
| `gotenberg.webhook.retryMaxWait`              | Set the maximum duration to wait before trying to call the webhook again         | `30s`       |
| `gotenberg.webhook.clientTimeout`             | Set the time limit for requests to the webhook                                   | `30s`       |
| `gotenberg.webhook.disable`                   | Disable the webhook feature                                                      | `false`     |
| `gotenberg.prometheus.collectInterval`        | Set the interval for collecting modules' metrics                                 | `1s`        |
| `gotenberg.prometheus.namespace`              | Set the namespace of modules' metrics                                            | `gotenberg` |
| `gotenberg.prometheus.disableCollect`         | Disable the collect of metrics                                                   | `false`     |
| `gotenberg.prometheus.disableRouteLogging`    | Disable the route logging                                                        | `false`     |
| `gotenberg.logging.format`                    | Specify the format of logging                                                    | `auto`      |
| `gotenberg.logging.level`                     | Choose the level of logging detail                                               | `info`      |
| `gotenberg.logging.fieldsPrefix`              | Prepend a specified prefix to each field in the logs                             | `""`        |
| `gotenberg.shutdown.duration`                 | Set the graceful shutdown duration                                               | `30s`       |

### Common Secret parameters

| Name                 | Description                                                        | Value |
| -------------------- | ------------------------------------------------------------------ | ----- |
| `secret.annotations` | Common annotations for the SMTP, HIBP, Admin and Database secrets  | `{}`  |
| `secret.labels`      | Common extra labels for the SMTP, HIBP, Admin and Database secrets | `{}`  |

### Ingress parameters

| Name                  | Description                                         | Value   |
| --------------------- | --------------------------------------------------- | ------- |
| `ingress.enabled`     | Whether to enable Ingress                           | `false` |
| `ingress.className`   | The IngressClass to use for the pod's ingress       | `""`    |
| `ingress.whitelist`   | A comma-separated list of IP addresses to whitelist | `""`    |
| `ingress.annotations` | Annotations for the Ingress resource                | `{}`    |
| `ingress.tls`         | A list of hostnames and secret names to use for TLS | `[]`    |
| `ingress.hosts`       | A list of hosts for the Ingress resource            | `[]`    |

### Service parameters

| Name                               | Description                                                                             | Value       |
| ---------------------------------- | --------------------------------------------------------------------------------------- | ----------- |
| `service.type`                     | The type of service to create                                                           | `ClusterIP` |
| `service.port`                     | The port to use on the service                                                          | `80`        |
| `service.nodePort`                 | The Node port to use on the service                                                     | `30080`     |
| `service.extraPorts`               | Extra ports to add to the service                                                       | `[]`        |
| `service.annotations`              | Annotations for the service resource                                                    | `{}`        |
| `service.labels`                   | Labels for the service resource                                                         | `{}`        |
| `service.externalTrafficPolicy`    | The external traffic policy for the service                                             | `Cluster`   |
| `service.internalTrafficPolicy`    | The internal traffic policy for the service                                             | `Cluster`   |
| `service.clusterIP`                | Define a static cluster IP for the service                                              | `""`        |
| `service.loadBalancerIP`           | Set the Load Balancer IP                                                                | `""`        |
| `service.loadBalancerClass`        | Define Load Balancer class if service type is `LoadBalancer` (optional, cloud specific) | `""`        |
| `service.loadBalancerSourceRanges` | Service Load Balancer source ranges                                                     | `[]`        |
| `service.externalIPs`              | Service External IPs                                                                    | `[]`        |
| `service.sessionAffinity`          | Session Affinity for Kubernetes service, can be "None" or "ClientIP"                    | `None`      |
| `service.sessionAffinityConfig`    | Additional settings for the sessionAffinity                                             | `{}`        |
| `service.ipFamilyPolicy`           | The ipFamilyPolicy                                                                      | `{}`        |

### RBAC parameters

| Name          | Description                      | Value  |
| ------------- | -------------------------------- | ------ |
| `rbac.create` | Whether to create RBAC resources | `true` |
| `rbac.rules`  | Extra rules to add to the Role   | `[]`   |

### Service Account parameters

| Name                         | Description                                                                 | Value   |
| ---------------------------- | --------------------------------------------------------------------------- | ------- |
| `serviceAccount.create`      | Whether a service account should be created                                 | `true`  |
| `serviceAccount.automount`   | Whether to automount the service account token                              | `false` |
| `serviceAccount.annotations` | Annotations to add to the service account                                   | `{}`    |
| `serviceAccount.name`        | A custom name for the service account, otherwise gotenberg.fullname is used | `""`    |
| `serviceAccount.secrets`     | A list of secrets mountable by this service account                         | `[]`    |

### Liveness Probe parameters

| Name                                | Description                                                 | Value   |
| ----------------------------------- | ----------------------------------------------------------- | ------- |
| `livenessProbe.enabled`             | Enable or disable the use of liveness probes                | `false` |
| `livenessProbe.initialDelaySeconds` | Configure the initial delay seconds for the liveness probe  | `5`     |
| `livenessProbe.timeoutSeconds`      | Configure the initial delay seconds for the liveness probe  | `1`     |
| `livenessProbe.periodSeconds`       | Configure the seconds for each period of the liveness probe | `10`    |
| `livenessProbe.successThreshold`    | Configure the success threshold for the liveness probe      | `1`     |
| `livenessProbe.failureThreshold`    | Configure the failure threshold for the liveness probe      | `10`    |

### Readiness Probe parameters

| Name                                 | Description                                                  | Value   |
| ------------------------------------ | ------------------------------------------------------------ | ------- |
| `readinessProbe.enabled`             | Enable or disable the use of readiness probes                | `false` |
| `readinessProbe.initialDelaySeconds` | Configure the initial delay seconds for the readiness probe  | `5`     |
| `readinessProbe.timeoutSeconds`      | Configure the initial delay seconds for the readiness probe  | `1`     |
| `readinessProbe.periodSeconds`       | Configure the seconds for each period of the readiness probe | `10`    |
| `readinessProbe.successThreshold`    | Configure the success threshold for the readiness probe      | `1`     |
| `readinessProbe.failureThreshold`    | Configure the failure threshold for the readiness probe      | `3`     |

### Startup Probe parameters

| Name                               | Description                                                | Value   |
| ---------------------------------- | ---------------------------------------------------------- | ------- |
| `startupProbe.enabled`             | Enable or disable the use of readiness probes              | `false` |
| `startupProbe.initialDelaySeconds` | Configure the initial delay seconds for the startup probe  | `5`     |
| `startupProbe.timeoutSeconds`      | Configure the initial delay seconds for the startup probe  | `1`     |
| `startupProbe.periodSeconds`       | Configure the seconds for each period of the startup probe | `10`    |
| `startupProbe.successThreshold`    | Configure the success threshold for the startup probe      | `1`     |
| `startupProbe.failureThreshold`    | Configure the failure threshold for the startup probe      | `10`    |

### PodDisruptionBudget parameters

| Name                               | Description                                           | Value  |
| ---------------------------------- | ----------------------------------------------------- | ------ |
| `podDisruptionBudget.enabled`      | Enable the pod disruption budget                      | `true` |
| `podDisruptionBudget.minAvailable` | The minimum amount of pods which need to be available | `1`    |

### Pod settings

| Name                | Description                                               | Value |
| ------------------- | --------------------------------------------------------- | ----- |
| `replicas`          | The amount of replicas Gotenberg deployment should create | `1`   |
| `resources`         | The resource limits/requests for the Gotenberg pod        | `{}`  |
| `volumes`           | Define volumes for the Gotenberg pod                      | `[]`  |
| `volumeMounts`      | Define volumeMounts for the Gotenberg pod                 | `[]`  |
| `initContainers`    | Define initContainers for the main Gotenberg server       | `[]`  |
| `nodeSelector`      | Node labels for pod assignment                            | `{}`  |
| `tolerations`       | Tolerations for pod assignment                            | `[]`  |
| `affinity`          | Affinity for pod assignment                               | `{}`  |
| `strategy`          | Specify a deployment strategy for the Gotenberg pod       | `{}`  |
| `podAnnotations`    | Extra annotations for the Gotenberg pod                   | `{}`  |
| `podLabels`         | Extra labels for the Gotenberg pod                        | `{}`  |
| `priorityClassName` | The name of an existing PriorityClass                     | `""`  |

### Security context settings

| Name                 | Description                                     | Value |
| -------------------- | ----------------------------------------------- | ----- |
| `podSecurityContext` | Security context settings for the Gotenberg pod | `{}`  |
| `securityContext`    | General security context settings for           | `{}`  |

### Autoscaling settings

| Name                                         | Description                                       | Value   |
| -------------------------------------------- | ------------------------------------------------- | ------- |
| `autoscaling`                                | Autoscaling settings                              | `{}`    |
| `autoscaling.enabled`                        | Enable or disable the HorizontalPodAutoscaler     | `false` |
| `autoscaling.minReplicas`                    | The minimum replicas to autoscale to              | `1`     |
| `autoscaling.maxReplicas`                    | The maximum replicas to autoscale to              | `10`    |
| `autoscaling.targetCPUUtilizationPercentage` | The CPU utilization at which to start autoscaling | `80`    |

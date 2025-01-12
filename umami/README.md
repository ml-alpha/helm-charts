# umami

Umami is a simple, fast, privacy-focused alternative to Google Analytics.

## TL;DR;

```console
helm repo add christianhuth https://charts.christianhuth.de
helm repo update
helm install my-release christianhuth/umami
```

## Introduction

Umami is a simple, fast, privacy-focused alternative to Google Analytics.

This chart bootstraps [Umami](https://github.com/umami-software/umami) on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.19+

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm repo add christianhuth https://charts.christianhuth.de
helm repo update
helm install my-release christianhuth/umami
```

These commands deploy Umami on the Kubernetes cluster in the default configuration. The [Values](#values) section lists the values that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall the `my-release` deployment:

```console
helm uninstall my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity settings for pod assignment |
| autoscaling.enabled | bool | `false` | Enable Horizontal POD autoscaling |
| autoscaling.maxReplicas | int | `100` | Maximum number of replicas |
| autoscaling.minReplicas | int | `1` | Minimum number of replicas |
| autoscaling.targetCPUUtilizationPercentage | int | `80` | Target CPU utilization percentage |
| autoscaling.targetMemoryUtilizationPercentage | int | `80` | Target Memory utilization percentage |
| database.databaseUrlKey | string | `""` | Key in the existing secret containing the database url |
| database.existingSecret | string | `""` | use an existing secret containing the database url. If none given, we will generate the database url by using the other values. The password for the database has to be set using `.Values.postgresql.auth.password`, `.Values.mysql.auth.password` or `.Values.externalDatabase.auth.password`. |
| externalDatabase.auth.database | string | `"mychart"` | Name of the database to use |
| externalDatabase.auth.password | string | `"mychart"` | Password to use |
| externalDatabase.auth.username | string | `"mychart"` | Name of the user to use |
| externalDatabase.hostname | string | `""` | Hostname of the database |
| externalDatabase.port | int | `5432` | Port used to connect to database |
| externalDatabase.type | string | `"postgresql"` | Type of database |
| extraEnv | list | `[]` | additional environment variables to be added to the pods. See https://umami.is/docs/environment-variables for a complete list of available variables. Most variables can be set under umami as well. |
| fullnameOverride | string | `""` | String to fully override `"umami.fullname"` |
| image.pullPolicy | string | `"Always"` | image pull policy |
| image.registry | string | `"ghcr.io"` | image registry |
| image.repository | string | `"umami-software/umami"` | image repository |
| image.tag | string | `"postgresql-v2.9.0"` | Overrides the image tag |
| imagePullSecrets | list | `[]` | If defined, uses a Secret to pull an image from a private Docker registry or repository. |
| ingress.annotations | object | `{}` | Additional annotations for the Ingress resource |
| ingress.className | string | `""` | IngressClass that will be be used to implement the Ingress |
| ingress.enabled | bool | `false` | Enable ingress record generation |
| ingress.hosts | list | see [values.yaml](./values.yaml) | An array with hosts and paths |
| ingress.tls | list | `[]` | An array with the tls configuration |
| mysql.auth.database | string | `"mychart"` | Name for a custom database to create |
| mysql.auth.password | string | `"mychart"` | Password for the custom user to create |
| mysql.auth.username | string | `"mychart"` | Name for a custom user to create |
| mysql.enabled | bool | `false` | enable MySQL™ subchart from Bitnami |
| nameOverride | string | `""` | Provide a name in place of `umami` |
| nodeSelector | object | `{}` | Node labels for pod assignment |
| podAnnotations | object | `{}` | Annotations to be added to pods |
| podLabels | object | `{}` | Labels to be added to pods |
| podSecurityContext | object | `{}` | pod-level security context |
| postgresql.auth.database | string | `"mychart"` | Name for a custom database to create |
| postgresql.auth.password | string | `"mychart"` | Password for the custom user to create |
| postgresql.auth.username | string | `"mychart"` | Name for a custom user to create |
| postgresql.enabled | bool | `true` | enable PostgreSQL™ subchart from Bitnami |
| replicaCount | int | `1` | Number of replicas |
| resources | object | `{}` | Resource limits and requests for the controller pods. |
| revisionHistoryLimit | int | `10` | The number of old ReplicaSets to retain |
| securityContext | object | `{"runAsGroup":65533,"runAsNonRoot":true,"runAsUser":1001}` | container-level security context |
| service.port | int | `3000` | Kubernetes port where service is exposed |
| service.type | string | `"ClusterIP"` | Kubernetes service type |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` | Toleration labels for pod assignment |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
helm install my-release -f values.yaml christianhuth/umami
```

## Upgrading the Chart

### To 3.0.0

This major updates the PostgreSQL subchart to its newest major, 14.0.0. [Here](https://github.com/bitnami/charts/tree/master/bitnami/postgresql#to-1400) you can find more information about the changes introduced in that version.

### To 2.0.0

This major updates the Docker Image to its newest major, 2.0.0. [Here](https://github.com/umami-software/umami/releases/tag/v2.0.0) you can find more information about the changes introduced in that version.

To upgrade from a previous version of the Helm Chart make sure to activate the database migration job with `umami.migration.v1v2.enabled`.

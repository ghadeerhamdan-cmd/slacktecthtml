# Helm Chart Name

This Helm chart deploys a service to Kubernetes with various options for customization.

## Installation

To install the chart with the release name `my-release`, run the following command:

```shell
helm install my-release path/to/chart
```

Replace `path/to/chart` with the path to the directory containing the chart.

## Configuration

The following table lists the configurable parameters of the chart and their default values.

| Parameter                      | Description                           | Default                                |
|--------------------------------|---------------------------------------|----------------------------------------|
| `nameOverride`                 | Overrides the default Helm chart name | `"generic-name"`                       |
| `fullnameOverride`             | Overrides the default full Helm chart name | `"generic-fullname"`                 |
| `cluster`                      | The name of the cluster where the chart is to be installed | `"generic-cluster"`                  |
| `environment`                  | The name of the environment where the chart is to be installed | `"generic-environment"`              |
| `commonLabels`                 | Labels to attach to all Kubernetes resources created by this chart | `{app_name: generic-app, app_env: generic-env, app_owner: generic-owner, app_squad: generic-squad}` |
| `serviceAccount.create`        | Whether a new service account should be created | `true`                                |
| `serviceAccount.name`          | The name of the service account to use | `"generic-service-account"`           |
| `deployment.enabled`           | Whether a deployment should be created | `true`                                |
| `deployment.replicaCount`      | The number of replicas to create | `1`                                    |
| `deployment.image.repository`  | The repository of the container image to use in the deployment | `"generic-image-repository"`         |
| `deployment.image.pullPolicy`  | The pull policy for the container image | `"IfNotPresent"`                      |
| `deployment.image.tag`         | The tag of the container image to use in the deployment | `"generic-tag"`                      |
| `service.enabled`              | Whether a service should be created | `true`                                |
| `service.type`                 | The type of service to create | `"ClusterIP"`                          |
| `service.name`                 | The name of the service to create | `"generic-service"`                   |
| `ingress.enabled`              | Whether an ingress should be created | `true`                                |
| `ingress.className`            | The ingress class name | `"nginx"`                              |
| `autoscaling.enabled`          | Whether horizontal pod autoscaler should be created | `true`                             |
| `autoscaling.minReplicas`      | The minimum number of replicas when autoscaling is enabled | `1`                               |
| `autoscaling.maxReplicas`      | The maximum number of replicas when autoscaling is enabled | `5`                               |
| `autoscaling.targetCPUScaling` | The target CPU utilization percentage to trigger autoscaling | `80`                              |
| `autoscaling.targetMemScaling` | The target memory utilization percentage to trigger autoscaling | `80`                            |

This is just a sample of some of the parameters you might find in the `values.yaml` file. There are many more that can be customized depending on your application's specific needs.

## Contributing

We welcome contributions! Please see our contributing guidelines for more details.

---

## Generic Examples

Here are some generic examples for each configuration parameter using a Helm cronjob chart as an example:

| Parameter                      | Example                                |
|--------------------------------|----------------------------------------|
| `nameOverride`                 | `"my-helm-chart"`                      |
| `fullnameOverride`             | `"my-full-helm-chart-name"`            |
| `cluster`                      | `"my-kubernetes-cluster"`              |
| `environment`                  | `"production"`                         |
| `commonLabels`                 | `{app_name: my-app, app_env: production, app_owner: my-owner, app_squad: my-squad}` |
| `serviceAccount.create`        | `true`                                 |
| `serviceAccount.name`          | `"my-service-account"`                 |
| `deployment.enabled`           | `true`                                 |
| `deployment.replicaCount`      | `2`                                    |
| `deployment.image.repository`  | `"nginx"`                              |
| `deployment.image.pullPolicy`  | `"Always"`                             |
| `deployment.image.tag`         | `"latest"`                             |
| `service.enabled`              | `true`                                 |
| `service.type`                 | `"NodePort"`                           |
| `service.name`                 | `"my-service"`                         |
| `ingress.enabled`              | `true`                                 |
| `ingress.className`            | `"nginx"`                              |
| `autoscaling.enabled`          | `true`                                 |
| `autoscaling.minReplicas`      | `2`                                    |
| `autoscaling.maxReplicas`      | `10`                                   |
| `autoscaling.targetCPUScaling` | `50`                                   |
| `autoscaling.targetMemScaling` | `60`                                   |

And here is an example of how to use this Helm chart:

```shell
$ helm install stg-cron-job .
NAME:   cold-fly
LAST DEPLOYED: Fri Feb  1 15:29:21 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/CronJob
NAME                    AGE
cold-fly-hello-world    1s
cold-fly-hello-ubuntu   1s
cold-fly-hello-env-var  1s

list cronjobs:

$ kubectl get cronjob
NAME                     SCHEDULE      SUSPEND   ACTIVE    LAST SCHEDULE   AGE
cold-fly-hello-env-var   * * * * *     False     0         23s             1m
cold-fly-hello-ubuntu    */5 * * * *   False     0         23s             1m
cold-fly-hello-world     * * * * *     False     0         23s             1m

list jobs:

$ kubectl get jobs
NAME                                DESIRED   SUCCESSFUL   AGE
cold-fly-hello-env-var-1549056600   1         1            45s【20†source】【21†source】.
```
Please note that these are just examples and you will need to replace the values with those that fit your specific use case.
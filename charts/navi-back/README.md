# 2GIS Navi-Back service

Use this Helm chart to deploy Navi-Back service, which is a part of 2GIS's [On-Premise Navigation services](https://docs.2gis.com/en/on-premise/navigation).

Read more about the On-Premise solution [here](https://docs.2gis.com/en/on-premise/overview).

> **Note:**
>
> All On-Premise services are beta, and under development.

See the [documentation](https://docs.2gis.com/en/on-premise/navigation) to learn about:

- Architecture of the service.

- Installing the service.

    When filling in the keys for `values-back.yaml` configuration file, refer to the documentation and the list of keys below.

- Updating the service.

## Values

### Docker Registry settings

| Name                  | Description                                                                             | Value |
| --------------------- | --------------------------------------------------------------------------------------- | ----- |
| `dgctlDockerRegistry` | Docker Registry endpoint where On-Premise services' images reside. Format: `host:port`. | `""`  |


### Common settings

| Name                 | Description                                                                                                                 | Value |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----- |
| `replicaCount`       | A replica count for the pod.                                                                                                | `1`   |
| `imagePullSecrets`   | Kubernetes image pull secrets.                                                                                              | `[]`  |
| `nameOverride`       | Base name to use in all the Kubernetes entities deployed by this chart.                                                     | `""`  |
| `fullnameOverride`   | Base fullname to use in all the Kubernetes entities deployed by this chart.                                                 | `""`  |
| `podAnnotations`     | Kubernetes [pod annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/).               | `{}`  |
| `podSecurityContext` | Kubernetes [pod security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/).              | `{}`  |
| `securityContext`    | Kubernetes [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/).                  | `{}`  |
| `nodeSelector`       | Kubernetes [node selectors](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector).         | `{}`  |
| `tolerations`        | Kubernetes [tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) settings.           | `[]`  |
| `affinity`           | Kubernetes pod [affinity settings](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity). | `{}`  |
| `labels`             | Custom labels to set to Deployment resource.                                                                                | `{}`  |


### Deployment settings

| Name               | Description | Value                       |
| ------------------ | ----------- | --------------------------- |
| `image.repository` | Repository  | `2gis-on-premise/navi-back` |
| `image.tag`        | Tag         | `6.12.0`                    |
| `image.pullPolicy` | Pull Policy | `IfNotPresent`              |


### Navi-Back application settings

| Name                          | Description                                                                                                                                                                                               | Value   |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `naviback.app_castle_host`    | URL of Navi-Castle service. <br> This URL should be accessible from all the pods within your Kubernetes cluster.                                                                                          |         |
| `naviback.eca_host`           | Domain name of the [Traffic Proxy service](https://docs.2gis.com/en/on-premise/traffic-proxy). <br> This URL should be accessible from all the pods within your Kubernetes cluster.                       |         |
| `naviback.forecast_host`      | URL of Traffic forecast service. See the [Traffic Proxy service](https://docs.2gis.com/en/on-premise/traffic-proxy). <br> This URL should be accessible from all the pods within your Kubernetes cluster. |         |
| `naviback.dm_sources_limit`   | Size limit for source matrices.                                                                                                                                                                           | `1000`  |
| `naviback.dm_targets_limit`   | Size limit for target matrices.                                                                                                                                                                           | `1000`  |
| `naviback.handlers_number`    | Number of HTTP handlers.                                                                                                                                                                                  | `1`     |
| `naviback.max_process_time`   | Maximum processing time limit in minutes.                                                                                                                                                                 | `600`   |
| `naviback.response_timelimit` | Maximum response time limit in minutes.                                                                                                                                                                   | `60`    |
| `naviback.request_timeout`    | Maximum request time limit in minutes.                                                                                                                                                                    | `60`    |
| `naviback.dump_result`        | Dump results in logs.                                                                                                                                                                                     | `false` |
| `naviback.dump_query`         | Dump queries in logs.                                                                                                                                                                                     | `false` |
| `naviback.dump_answer`        | Dump answers in logs.                                                                                                                                                                                     | `false` |
| `naviback.logLevel`           | Logging level, one of: Verbose, Info, Warning, Error, Fatal.                                                                                                                                              | `Info`  |
| `naviback.indices`            | List of dynamic indices kill switches.                                                                                                                                                                    |         |


### Service account settings

| Name                         | Description                                                                                                             | Value  |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------ |
| `serviceAccount.create`      | Specifies whether a service account should be created.                                                                  | `true` |
| `serviceAccount.annotations` | Annotations to add to the service account.                                                                              | `{}`   |
| `serviceAccount.name`        | The name of the service account to use. If not set and create is true, a name is generated using the fullname template. | `""`   |


### Service settings

| Name                  | Description                                                                                                                    | Value       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------- |
| `service.type`        | Kubernetes [service type](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types). | `ClusterIP` |
| `service.port`        | Service port.                                                                                                                  | `80`        |
| `service.annotations` | Kubernetes [service annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/).              | `{}`        |
| `service.labels`      | Kubernetes [service labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).                        | `nil`       |


### Kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) settings

| Name                    | Description                            | Value            |
| ----------------------- | -------------------------------------- | ---------------- |
| `ingress.enabled`       | If Ingress is enabled for the service. | `false`          |
| `ingress.hosts[0].host` | Hostname for the Ingress service.      | `navi-back.host` |


### Limits

| Name                            | Description                     | Value    |
| ------------------------------- | ------------------------------- | -------- |
| `resources.requests.cpu`        | A CPU request.                  | `500m`   |
| `resources.requests.memory`     | A memory request.               | `1024Mi` |
| `resources.limits.cpu`          | A CPU limit.                    | `2`      |
| `resources.limits.memory`       | A memory limit.                 | `4000Mi` |
| `testResources`                 | **Limits for test connection.** |          |
| `testResources.requests.cpu`    | A CPU request.                  | `100m`   |
| `testResources.requests.memory` | A memory request.               | `100Mi`  |
| `testResources.limits.cpu`      | A CPU limit.                    | `100m`   |
| `testResources.limits.memory`   | A memory limit.                 | `100Mi`  |


### Kubernetes [Horizontal Pod Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) settings

| Name                                      | Description                                                                                                                                                          | Value   |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `hpa.enabled`                             | If HPA is enabled for the service.                                                                                                                                   | `false` |
| `hpa.minReplicas`                         | Lower limit for the number of replicas to which the autoscaler can scale down.                                                                                       | `1`     |
| `hpa.maxReplicas`                         | Upper limit for the number of replicas to which the autoscaler can scale up.                                                                                         | `100`   |
| `hpa.scaleDownStabilizationWindowSeconds` | Scale-down window.                                                                                                                                                   | `""`    |
| `hpa.scaleUpStabilizationWindowSeconds`   | Scale-up window.                                                                                                                                                     | `""`    |
| `hpa.targetCPUUtilizationPercentage`      | Target average CPU utilization (represented as a percentage of requested CPU) over all the pods; if not specified the default autoscaling policy will be used.       | `80`    |
| `hpa.targetMemoryUtilizationPercentage`   | Target average memory utilization (represented as a percentage of requested memory) over all the pods; if not specified the default autoscaling policy will be used. | `""`    |


### Vertical scaling settings

| Name                      | Description                     | Value   |
| ------------------------- | ------------------------------- | ------- |
| `verticalscaling.enabled` | If vertical scaling is enabled. | `false` |


### Kubernetes [Vertical Pod Autoscaling](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/README.md) settings

| Name                    | Description                                                                                                  | Value   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ | ------- |
| `vpa.enabled`           | If VPA is enabled for the service.                                                                           | `false` |
| `vpa.updateMode`        | VPA [update mode](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#quick-start). | `Auto`  |
| `vpa.minAllowed.cpu`    | Lower limit for the number of CPUs to which the autoscaler can scale down.                                   |         |
| `vpa.minAllowed.memory` | Lower limit for the RAM size to which the autoscaler can scale down.                                         |         |
| `vpa.maxAllowed.cpu`    | Upper limit for the number of CPUs to which the autoscaler can scale up.                                     |         |
| `vpa.maxAllowed.memory` | Upper limit for the RAM size to which the autoscaler can scale up.                                           |         |


### Kubernetes [Pod Disruption Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/#pod-disruption-budgets) settings

| Name                 | Description                                          | Value   |
| -------------------- | ---------------------------------------------------- | ------- |
| `pdb.enabled`        | If PDB is enabled for the service.                   | `false` |
| `pdb.minAvailable`   | How many pods must be available after the eviction.  | `""`    |
| `pdb.maxUnavailable` | How many pods can be unavailable after the eviction. | `1`     |


### Kafka settings for interacting with Distance Matrix Async Service

| Name                                             | Description                                                                    | Value                   |
| ------------------------------------------------ | ------------------------------------------------------------------------------ | ----------------------- |
| `kafka.enabled`                                  | If the Kafka is enabled.                                                       | `false`                 |
| `kafka.server`                                   | Kafka hostname or IP address.                                                  | `async-matrix-api.host` |
| `kafka.port`                                     | Kafka port.                                                                    | `9092`                  |
| `kafka.groupId`                                  | Navi-Back service group identifier.                                            | `test_id`               |
| `kafka.user`                                     | Username for accessing the Kafka server.                                       | `kafkauser`             |
| `kafka.password`                                 | Password for accessing the Kafka server.                                       | `kafkapassword`         |
| `kafka.mechanism`                                | Authentication mechanism.                                                      | `SCRAM-SHA-512`         |
| `kafka.protocol`                                 | Kafka protocol.                                                                | `SASL_SSL`              |
| `kafka.distanceMatrix`                           | **Settings for interacting with Distance Matrix Async service.**               |                         |
| `kafka.distanceMatrix.taskTopic`                 | Name of the topic for receiving new tasks from Distance Matrix Async API.      | `request_topic`         |
| `kafka.distanceMatrix.cancelTopic`               | Name of the topic for canceling or receiving information about finished tasks. | `cancel_topic`          |
| `kafka.distanceMatrix.statusTopic`               | Name of the topic for receiving task status information.                       | `service_message_bus`   |
| `kafka.distanceMatrix.updateTaskStatusPeriodSec` | Update period for task statuses.                                               | `120`                   |
| `kafka.distanceMatrix.messageExpiredPeriodSec`   | Update period for task cancellations.                                          | `3600`                  |
| `kafka.distanceMatrix.requestDownloadTimeoutSec` | Timeout for downloading request data.                                          | `20`                    |
| `kafka.distanceMatrix.responseUploadTimeoutSec`  | Timeout for uploading response data.                                           | `40`                    |


### S3-compatible storage settings for interacting with Distance Matrix Async Service

| Name         | Description                             | Value                     |
| ------------ | --------------------------------------- | ------------------------- |
| `s3.enabled` | if S3 storage is enabled.               | `false`                   |
| `s3.url`     | S3 endpoint URL.                        | `async-matrix-s3.host:80` |
| `s3.bucket`  | S3 bucket name.                         | `samplebucket`            |
| `s3.keyId`   | S3 access key for accessing the bucket. | `sampleid`                |
| `s3.key`     | S3 secret key for accessing the bucket. | `samplekey`               |


## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| 2gis | <on-premise@2gis.com> | <https://github.com/2gis> |

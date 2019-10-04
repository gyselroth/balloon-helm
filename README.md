# balloon Helm Chart

## Prerequisites Details

* Kubernetes 1.9+
* PV support on the underlying infrastructure

## Chart Details

This chart implements [balloon](https://github.com/gyselroth/balloon) with all necessary and optional features as sub charts.
This charts includes the following sub charts:

* balloon server
* balloon server proxy
* balloon web ui
* balloon jobs
* browserless-chrome

The following sub charts get installed as dependencies from helm stable if enabled (which is the default):

* [MongoDB replicaSet](https://github.com/helm/charts/tree/master/stable/mongodb-replicaset)
* [Elasticsearch](https://github.com/helm/charts/tree/master/stable/elasticsearch)
* [ClamAV](https://github.com/helm/charts/tree/master/stable/clamav)
* [Collabora Code (Collab)](https://github.com/helm/charts/tree/master/stable/collabora-code)
* [Collabora Code (Document conversions)](https://github.com/helm/charts/tree/master/stable/collabora-code)

balloon sub charts do not require persistent volumes however the balloon server is dependent on MongoDB and therefore a pv infrastructure is required
which also applies to the optional elasticsearch integration.

## Installing the Chart

To install the chart with the release name `my-release`:

``` console
helm repo add stable https://gyselroth.github.io/balloon-helm
helm install --name my-release stable/balloon
```

## Configuration

The following table lists the configurable parameters of the balloon sub charts and their default values.

(Note: Only the configuration for the balloon server sub chart is documented so far)


| Parameter                           | Description                                                               | Default                                             |
| ----------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `balloon.replicaCount`              | Number of replicas in the replica set                                     | `2`                                                 |
| `balloon.fullnameOverrride`         | Override deployment name                                                  | `""`                                                |
| `balloon.nameOverride`              | Override deployment name                                                  | `""`                                                |
| `balloon.url`                       | The URL of balloon under which the server is reachable from outside       | `https://balloon.local`                             |
| `balloon.image.repository`          | Image repository and name                                                 | `gyselroth/balloon`                                 |
| `balloon.image.tag`                 | Image tag for the install container                                       | `2.6.6`                                             |
| `balloon.image.pullPolicy`          | Image pull policy for the init container that establishes the replica set | `IfNotPresent`                                      |
| `balloon.service.type`              | Service Type                                                              | `ClusterIP`                                         |
| `balloon.service.port`              | Service Port (balloon < v3 is executed using PHP-FPM)                     | `9000`                                              |
| `balloon.extraLabels`               | Extra labels                                                              | `{}`                                                |
| `balloon.extraVars`                 | Extra env variables                                                       | `{}`                                                |
| `balloon.nodeSelector`              | Custom node selector                                                      | `""`                                                |
| `balloon.tolerations`               | Tolerations                                                               | `[]`                                                |
| `balloon.resources`                 | Pod resource requests and limits                                          | `{}`                                                |
| `balloon.affinity`                  | Affinitiy                                                                 | `{}`                                                |

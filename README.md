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

```console
helm repo add balloon https://gyselroth.github.io/balloon-helm/stable
helm install balloon/balloon --name my-release --namespace mynamespace
```

Example deployment with ingress/tls enabled:

```console
helm install balloon/balloon --name my-release --namespace mynamespace \
    --set balloon-proxy.ingress.enabled=true \
    --set balloon-web.ingress.enabled=true \
    --set balloon-proxy.ingress.host=balloon.local \
    --set balloon-web.ingress.host=balloon.local \
    --set balloon-web.ingress.tls[0].secretName=tls-balloon.local \
    --set balloon-proxy.ingress.tls[0].secretName=tls-balloon.local \
    --set balloon.url=https://balloon.local
```

The balloon should now be installed and available. You may authenticate using the default admin user:

Username: admin<br/>
Password: admin<br/>

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
| `balloon.image.tag`                 | Image tag for the install container                                       | `2.6.7`                                             |
| `balloon.image.pullPolicy`          | Image pull policy for the init container that establishes the replica set | `IfNotPresent`                                      |
| `balloon.service.type`              | Service Type                                                              | `ClusterIP`                                         |
| `balloon.service.port`              | Service Port (balloon < v3 is executed using PHP-FPM)                     | `9000`                                              |
| `balloon.extraLabels`               | Extra labels                                                              | `{}`                                                |
| `balloon.extraVars`                 | Extra env variables                                                       | `{}`                                                |
| `balloon.nodeSelector`              | Custom node selector                                                      | `""`                                                |
| `balloon.tolerations`               | Tolerations                                                               | `[]`                                                |
| `balloon.resources`                 | Pod resource requests and limits                                          | `{}`                                                |
| `balloon.affinity`                  | Affinitiy                                                                 | `{}`                                                |


## Release new charts

Upgrade the sub charts first which have changes.

Pack chart and update dependencies:
```
helm package -u -d stable/balloon/ stable/balloon/
```

Update repository index:
```
helm repo index stable/
```

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
helm repo add balloon-stable https://gyselroth.github.io/balloon-helm/stable
helm install balloon-stable/balloon --name my-release --namespace mynamespace
```

Example deployment with ingress/tls enabled and 200GB MongoDB/Elasticsearch storage:

```console
helm install balloon-stable/balloon --name my-release --namespace mynamespace \
    --set balloon-proxy.ingress.enabled=true \
    --set balloon-web.ingress.enabled=true \
    --set balloon-proxy.ingress.host=balloon-api.local \
    --set balloon-web.ingress.host=balloon-api.local \
    --set balloon-web.ingress.tls[0].secretName=tls-balloon-api.local \
    --set balloon-proxy.ingress.tls[0].secretName=tls-balloon-api.local \
    --set balloon-api.url=https://balloon-api.local \
    --set lool-collab.collabora.domain=https://balloon-api.local \
    --set mongodb.persistentVolume.size=200Gi \
    --set elasticsearch.data.persistence.size=75Gi \
    --set elasticsearch.data.replicas=3
```

The balloon should now be installed and available. You may authenticate using the default admin user:

Username: admin<br/>
Password: admin<br/>

## Configuration

The following table lists the configurable parameters of the balloon sub charts and their default values.

(Note: Only the configuration for the balloon server sub chart is documented so far)


| Parameter                           | Description                                                               | Default                                             |
| ----------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `balloon-api.replicaCount`              | Number of replicas in the replica set                                     | `2`                                                 |
| `balloon-api.fullnameOverrride`         | Override deployment name                                                  | `""`                                                |
| `balloon-api.nameOverride`              | Override deployment name                                                  | `""`                                                |
| `balloon-api.url`                       | The URL of balloon under which the server is reachable from outside       | `https://balloon-api.local`                             |
| `balloon-api.image.repository`          | Image repository and name                                                 | `gyselroth/balloon`                                 |
| `balloon-api.image.tag`                 | Image tag for the install container                                       | `2.6.7`                                             |
| `balloon-api.image.pullPolicy`          | Image pull policy for the init container that establishes the replica set | `IfNotPresent`                                      |
| `balloon-api.service.type`              | Service Type                                                              | `ClusterIP`                                         |
| `balloon-api.service.port`              | Service Port (balloon < v3 is executed using PHP-FPM)                     | `9000`                                              |
| `balloon-api.extraLabels`               | Extra labels                                                              | `{}`                                                |
| `balloon-api.extraVars`                 | Extra env variables                                                       | `{}`                                                |
| `balloon-api.nodeSelector`              | Custom node selector                                                      | `""`                                                |
| `balloon-api.tolerations`               | Tolerations                                                               | `[]`                                                |
| `balloon-api.resources`                 | Pod resource requests and limits                                          | `{}`                                                |
| `balloon-api.affinity`                  | Affinitiy                                                                 | `{}`                                                |


## Installing pre releases

Besides the stable repository there is a repository which holds unstable balloon charts. 
Unstable charts also include all balloon pre-releases (Usually alpha and beta versions).

To install an unstable chart with the release name `my-release`:

```console
helm repo add balloon-unstable https://gyselroth.github.io/balloon-helm/unstable
helm install balloon-unstable/balloon --name my-release --namespace mynamespace
```

## Release new charts

Upgrade the sub charts first which have changes.

Pack chart and update dependencies:
```
helm package -d stable stable/balloon
```

Checkout gh-pages and rebuild index
```
git checkout gh-pages
git add stable/*.tgz
helm repo index stable/
git commit .
git checkout master
```

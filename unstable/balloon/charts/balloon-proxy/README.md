balloon-proxy
=============
Nginx proxy for balloon php-fpm

Current chart version is `1.0.0`

Source code can be found [here](https://github.com/gyselroth/balloon)



## Chart Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| balloon-api.service.name | string | `nil` |  |
| balloon-api.service.port | int | `9000` |  |
| extraLabels | object | `{}` |  |
| extraVars | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| hpa.averageUtilization | int | `50` |  |
| hpa.averageValue | int | `300` |  |
| hpa.enabled | bool | `false` |  |
| hpa.maxReplicas | int | `20` |  |
| hpa.metricName | string | `"balloon_http_requests"` |  |
| hpa.metricType | string | `"Object"` |  |
| hpa.minReplicas | int | `2` |  |
| hpa.objectApiVersion | string | `"v1"` |  |
| hpa.objectKind | string | `"Service"` |  |
| hpa.objectName | string | `nil` |  |
| hpa.resourceName | string | `"cpu"` |  |
| hpa.targetType | string | `"AverageValue"` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"nginxinc/nginx-unprivileged"` |  |
| image.tag | string | `"1-alpine"` |  |
| ingress.annotations | object | `{}` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].name | string | `"balloon.local"` |  |
| livenessProbe.enabled | bool | `true` |  |
| livenessProbe.failureThreshold | int | `5` |  |
| livenessProbe.initialDelaySeconds | int | `5` |  |
| livenessProbe.periodSeconds | int | `5` |  |
| livenessProbe.successThreshold | int | `1` |  |
| livenessProbe.timeoutSeconds | int | `5` |  |
| nameOverride | string | `""` |  |
| nginx.fastcgi_read_timeout | int | `300` |  |
| nodeSelector | object | `{}` |  |
| readinessProbe.enabled | bool | `true` |  |
| readinessProbe.failureThreshold | int | `5` |  |
| readinessProbe.initialDelaySeconds | int | `5` |  |
| readinessProbe.periodSeconds | int | `5` |  |
| readinessProbe.successThreshold | int | `1` |  |
| readinessProbe.timeoutSeconds | int | `1` |  |
| replicaCount | int | `2` |  |
| resources | object | `{}` |  |
| securityContext.allowPrivilegeEscalation | bool | `false` |  |
| securityContext.capabilities.drop[0] | string | `"all"` |  |
| securityContext.enabled | bool | `true` |  |
| securityContext.readOnlyRootFilesystem | bool | `true` |  |
| securityContext.runAsGroup | int | `1000` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1000` |  |
| service.port | int | `8080` |  |
| service.type | string | `"ClusterIP"` |  |
| tolerations | list | `[]` |  |

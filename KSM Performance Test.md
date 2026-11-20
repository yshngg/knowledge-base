## Background

[Kubernetes v1.36: Server-Side Sharded List and Watch](https://kubernetes.io/blog/2026/05/06/kubernetes-v1-36-server-side-sharded-list-and-watch/)

https://github.com/kubernetes/kube-state-metrics/pull/3127

## Start Minikube

```bash
minikube start --driver kvm2 --nodes 3 --extra-config=apiserver.feature-gates=ShardedListAndWatch=true
```

[[kvm2]]

## Install Kube-Prometheus-Stack

_values.yaml_

```yaml
## Deploy a Prometheus instance
##
prometheus:
  ## Settings affecting prometheusSpec
  ## ref: https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api-reference/api.md#prometheusspec
  ##
  prometheusSpec:
    ## Resource limits & requests
    ##
    resources:
      requests:
        memory: 2Gi

## Configuration for kube-state-metrics subchart
##
kube-state-metrics:
  image:
    registry: docker.io
    repository: yshngg/kube-state-metrics
    tag: "main-amd64"
    # tag: "server-side-sharding-amd64"

  # If set to true, this will deploy kube-state-metrics as a StatefulSet and the data
  # will be automatically sharded across <.Values.replicas> pods using the built-in
  # autodiscovery feature: https://github.com/kubernetes/kube-state-metrics#automated-sharding
  # This is an experimental feature and there are no stability guarantees.
  autosharding:
    enabled: true

  replicas: 2
```

```bash
helm upgrade --install --namespace monitoring --create-namespace -f ./values.yaml --reuse-values kube-prometheus-stack oci://ghcr.io/prometheus-community/charts/kube-prometheus-stack
```

https://github.com/prometheus-community/helm-charts/tree/kube-prometheus-stack-93.4.0/charts/kube-prometheus-stack

```console
Release "kube-prometheus-stack" does not exist. Installing it now.
Pulled: ghcr.io/prometheus-community/charts/kube-prometheus-stack:93.4.0
Digest: sha256:94e3d2a538240f781e171783d8f68d5f83f909496d8bbcd84993592b0b5867a1
NAME: kube-prometheus-stack
LAST DEPLOYED: Wed Nov 18 19:52:06 2026
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=kube-prometheus-stack"

Get Grafana 'admin' user password by running:

  kubectl --namespace monitoring get secrets kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

Access Grafana local instance:

  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=kube-prometheus-stack" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000

Get your grafana admin user password by running:

  kubectl get secret --namespace monitoring -l app.kubernetes.io/component=admin-secret -o jsonpath="{.items[0].data.admin-password}" | base64 --decode ; echo


Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```

## Performance Testing

### Without Server Side Sharding

```bash
kubectl create deployment pause --image registry.k8s.io/pause:latest --replicas=100

kubectl rollout status deployment pause

kubectl delete deployment pause
```

### With Server Side Sharding

_values.yaml_

```bash
helm upgrade --namespace monitoring --reuse-values --set kube-state-metrics.image.tag=server-side-sharding-amd64  kube-prometheus-stack oci://ghcr.io/prometheus-community/charts/kube-prometheus-stack
```

## References

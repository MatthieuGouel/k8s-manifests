# Docker registry manifest

This is a deployment of a private Docker registry on Kubernetes.

## Install the docker registry

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/registry/registry.yml
```

## Uninstall the docker registry

```
kubectl delete -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/registry/registry.yml
```

## sources

* https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/registry

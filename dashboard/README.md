# Dashboard manifest

The Kubernetes dashboard is really cool to monitor your cluster.

Note that is the "insecure" version of the deployment because it uses HTTP into the cluster.

## Install dashboard

As usual.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/dashboard/dashboard.yml
```

## Sources

This manifest is written based of my researches in several websites.  
Here is the sources which have helped me the most (the list may be not exclusive).

* https://github.com/kubernetes/dashboard/wiki/Installation#recommended-setup
* https://github.com/kubernetes/dashboard/issues/574
* https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

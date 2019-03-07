# nginx-health manifest

This is just a deployment of a nginx service.  
It helps to verify the good behavior of Traefik and give an example of using Traefik as an Ingress.

## Install nginx-health

Be sure to have Traefik installed and the certificate stored as a secret (see [here](https://github.com/MatthieuGouel/k8s-manifests/blob/master/traefik/README.md))

Then, simply apply the following manifest.

```
kubectl apply -f https://raw.githubusercontent.com/matthieugouel/k8s-manifests/master/nginx-health/nginx-health.yml
```

## Sources

This manifest is written based of my researches in several websites.
Here is the sources which have helped me the most (the list may be not exclusive).

* https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/

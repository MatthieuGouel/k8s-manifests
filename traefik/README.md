# Traefik manifest

## Generate certificate

This Traefik manifest use a certificate to validate HTTPS connection.

This is just an example but here I use a self-signed wildcard certificate : \*.home.local

In order to generate a proper self-signed wildcard certificate, you can follow this Stack Overflow post [here](https://stackoverflow.com/questions/7580508/getting-chrome-to-accept-self-signed-localhost-certificate/43666288#43666288).

Or you can directly a certificate signed from [Let's encrypt](https://letsencrypt.org/).

## How to install Traefik

Before actually install Traefik, you must store the certificate and the key in a Kubernetes secret.

Because a secret is bound to a namespace, we first need to create the namespace traefik.

```
kubectl create namespace traefik
```

Then, wa store the certificate as a Kubernetes secret.
An example with my self-signed certificate.

```
kubectl create secret tls home --cert=home.local.crt --key=device.key -n traefik
```

You can notice that the secret's name is "home" which is the name used everywhere else in the other manifests.

Then, simply apply the Traefik configuration file.

```
kubectl apply -f https://github.com/MatthieuGouel/k8s-manifests/blob/master/traefik/traefik.yml
```

## Use Traefik with your apps

Now, you have to add an Ingress in your app's manifests if you want to reach them through Traefik.

For instance, this is the ingress of the [nginx-health](https://github.com/MatthieuGouel/k8s-manifests/tree/master/nginx-health) app.

```
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginx-health
  namespace: health
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  tls:
  - secretName: home
  rules:
  - host: health.home.local
    http:
      paths:
      - path: /
        backend:
          serviceName: nginx-health
          servicePort: http
```

You can see that we use the "home" secret so the HTTPS front-end connection (health.home.local) is correctly signed.

## Sources

This manifest is written based of my researches in several websites.
Here is the sources which have helped me the most (the list may be not exclusive).

* https://docs.traefik.io/user-guide/kubernetes/
* http://blog.wescale.fr/2017/09/04/kubernetes-utiliser-traefik-comme-loadbalancer/
* https://medium.com/@patrickeasters/using-traefik-with-tls-on-kubernetes-cb67fb43a948

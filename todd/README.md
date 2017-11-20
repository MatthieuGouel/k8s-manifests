# ToDD manifests

Installing ToDD on Kubernetes is pretty strait-forward.

# Install ToDD

First, create the namespace.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/namespace.yml
```

Then, etcd.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/etcd.yml
```

Then, rabbitmq.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/rabbitmq.yml
```

Then, influxdb.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/influxdb.yml
```

Then, grafana.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/grafana.yml
```

Then, todd-server.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/todd-server.yml
```

Then todd-agent.

```
kubectl apply -f https://raw.githubusercontent.com/MatthieuGouel/k8s-manifests/master/todd/todd-agent.yml
```

You can set multiple replicas of todd-agent if you want to.

# Uninstall ToDD

Simply delete the namespace.

```
kubectl delete namespace todd
```

# Notes

I have a problem when I use ToDD API outside of the cluster because of the http to https redirection option of Traefik.  
Everything works great with a GET method but with a POST (create a group for instance), I have the following error server-side of ToDD.

```
2017/11/20 23:25:32 http: panic serving 10.244.0.0:47068: unexpected end of JSON input
goroutine 4277 [running]:
net/http.(*conn).serve.func1(0xc8200a7080, 0x7fee68aa74e0, 0xc820028148)
	/usr/local/go/src/net/http/server.go:1287 +0xb5
github.com/toddproject/todd/api/server.ToDDApi.CreateObject(0xc820011c60, 0x7, 0xc820011f00, 0x4, 0xc8200ef160, 0x7, 0xc8200ef400, 0x4, 0xc8200eee30, 0x8, ...)
	/go/src/github.com/toddproject/todd/api/server/object.go:69 +0x1dc
github.com/toddproject/todd/api/server.(ToDDApi).CreateObject-fm(0x7fee6aae09c0, 0xc8200cea50, 0xc82048d260)
	/go/src/github.com/toddproject/todd/api/server/server.go:45 +0x68
net/http.HandlerFunc.ServeHTTP(0xc82015e900, 0x7fee6aae09c0, 0xc8200cea50, 0xc82048d260)
	/usr/local/go/src/net/http/server.go:1422 +0x3a
net/http.(*ServeMux).ServeHTTP(0xc820013110, 0x7fee6aae09c0, 0xc8200cea50, 0xc82048d260)
	/usr/local/go/src/net/http/server.go:1699 +0x17d
net/http.serverHandler.ServeHTTP(0xc8200f6660, 0x7fee6aae09c0, 0xc8200cea50, 0xc82048d260)
	/usr/local/go/src/net/http/server.go:1862 +0x19e
net/http.(*conn).serve(0xc8200a7080)
	/usr/local/go/src/net/http/server.go:1361 +0xbee
created by net/http.(*Server).Serve
	/usr/local/go/src/net/http/server.go:1910 +0x3f6
```

And a 502 error client-side :

```
502 Bad Gateway
```

So, for now, in order to use the ToDD API outside of the cluster, I must deactivate the http to https redirection in Traefik.  
To do that, delete this two lines of the config.toml configuration file of Traefik :

```
[entryPoints.http.redirect]
entryPoint = "https"
```

## Sources

This manifest is written based of my researches in several websites.  
Here is the sources which have helped me the most (the list may be not exclusive).

* https://github.com/toddproject/todd

* https://github.com/coreos/etcd/blob/master/hack/kubernetes-deploy/etcd.yml
* https://github.com/sgotti/k8s-persistent-etcd/blob/master/resources/etcd.yaml

* https://github.com/nanit/kubernetes-rabbitmq-cluster/blob/master/kube/stateful.set.yml

* https://github.com/containous/traefik/issues/541

# Jaeger

### Create Namespace
```
kubectl create namespace observability
```

### Install Cert-manager
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```

### Install Operator
```
kubectl create -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.38.0/jaeger-operator.yaml -n observability
```

### Deploy Jaeger
```
kubectl --namespace observability create -f jaeger.yaml
```

### Port forwarding
```
kubectl port-forward --namespace observability  $(kubectl get pods --namespace observability -l "app.kubernetes.io/instance=jaeger,app.kubernetes.io/component=all-in-one" -o jsonpath="{.items[0].metadata.name}")  16686:16686
```

### UI Access
http://localhost:16686/
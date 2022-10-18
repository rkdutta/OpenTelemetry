# Jaeger

### Install Operator
```
kubectl create -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.38.0/jaeger-operator.yaml -n observability
```

### Deploy Jaeger
```
kubectl create -f jaeger.yaml
```

### Port forwarding
```
kubectl port-forward --namespace default  $(kubectl get pods --namespace default -l "app.kubernetes.io/instance=jaeger,app.kubernetes.io/component=all-in-one" -o jsonpath="{.items[0].metadata.name}")  16686:16686
```

### UI Access
http://localhost:16686/
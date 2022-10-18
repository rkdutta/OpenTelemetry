# ZIPKIN

### Create Namespace
```
kubectl create namespace observability
```

### Install
```
kubectl --namespace observability create -f zipkin.yaml
```

### Port forwarding
```
kubectl port-forward --namespace observability $(kubectl get pods --namespace observability -l=app=zipkin -o jsonpath="{.items[0].metadata.name}") 9411:9411
```

### UI Access
http://localhost:9411/
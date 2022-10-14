# ZIPKIN

### Install
```
kubectl create -f zipkin.yaml
```

### Port forwarding
```
kubectl port-forward --namespace default $(kubectl get pods --namespace default -l=app=zipkin -o jsonpath="{.items[0].metadata.name}") 9411:9411
```

### UI Access
http://localhost:9411/
# Testing APP
Infra. required on for app instrumentation in each namespace.

### testing
```
kubectl -n app-a exec -it testpod -- curl http://front-service:8080/customerDetails/1
```

MAC Laptops: to repeat 10 requests:
```
repeat 10 kubectl -n app-a exec -it testpod -- curl http://front-service:8080/customerDetails/1
```

### Jaeger UI Access
http://localhost:16686/

### Zipkin UI Access
http://localhost:9411/
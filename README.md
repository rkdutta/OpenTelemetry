
# Step by Step Demo for OpenTelemtry  

In this demo we are going to setup a kubernetes cluster using kind and deploy 2 spring-boot apis. Then we will implement tracing using OpenTelemetry. The reliability platform setup is loosly coupled and distributed. In the end you should be able to see tracing information from the kubernetes core components and apis. 

## 00 - Setting up a kubernetes cluster using kind on Docker
Command:
```text
kind create cluster --config 00-kind-cluster-setup/kind-cluster-1.yaml --name cluster-1
```
```text
1. One Control plane node
2. One worker node
```

## 01 - Deploy prerequisites
```text
1. cert-manager
2. namespaces (observability & project app-a)
```
Command:
```
kubectl apply -f 01-prerequisites-deploy/
```

## 02 - Deploy OpenTelemetry Operator
Command:
```
kubectl apply -f 02-otel-operator-deploy/
```

## 03 - Deploy OpenTelemetry Central Platform
```text
1. OpenTelemetry Collector as DaemonSet (to collect core kubernetes traces)
2. OpenTelemetry Collector as a remote deployment (to collect traces from all sources and send to a sink) 
```
Command:
```
kubectl apply -f 03-otel-central-platform/
```

## 04-06 -  Deploy Sinks (Jaeger & Zipkin)
```text
1. Jaeger Operator
2. Jaeger Deployment
3. Zipkin Deployment
```
Command:
```
kubectl apply -f 04-jaeger-operator-deploy/
kubectl apply -f 05-jaeger-deploy/
kubectl apply -f 06-zipkin-deploy/
```

## 07 - Deploy project platform
```text
1. instrumentation config (e.g. java)
2. OpenTelemetry local collector 
3. OpenTelemetry sidecar collector
```
Command:
```
kubectl apply -f 07-otel-project-platform/
```

## 08 - Deploy applications

### Microservice Architecture
```
 --------------------             ------------------            ------------------      
| Client(UI/Browser) | =======> | Front api service | =======> | Customer service |   
 --------------------             ------------------            ------------------
```
###### Reference: [customer service](https://github.com/rkdutta/otel-demo-customer-service)
###### Reference: [Front api service](https://github.com/rkdutta/otel-demo-api-service)

```text
1. Deploy customer service
2. Deploy back to front api service
```
Command:
```
kubectl apply -f 08-app-deploy/
```

## 09 - Deploy testing pod
Command:
```
kubectl apply -f 09-testpods-deploy/
```

## 10 - Port Forwarding
Jaeger
```text
kubectl port-forward --namespace observability  $(kubectl get pods --namespace observability -l "app.kubernetes.io/instance=jaeger,app.kubernetes.io/component=all-in-one" -o jsonpath="{.items[0].metadata.name}")  16686:16686 &
```
Zipkin
```text
kubectl port-forward --namespace observability $(kubectl get pods --namespace observability -l=app=zipkin -o jsonpath="{.items[0].metadata.name}") 9411:9411 &
```

## 11 - Access the Tracing Sinks (Jaeger & Zipkin)
### Jaeger UI Access
http://localhost:16686/

### Zipkin UI Access
http://localhost:9411/
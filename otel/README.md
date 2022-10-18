https://opentelemetry.io/docs/k8s-operator/
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
kubectl apply -f https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml

kubectl create ns otel

Dockerfile is an example of creating a specific otel agent with a specific java version and custom settings

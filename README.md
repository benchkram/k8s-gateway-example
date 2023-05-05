# How To:  Replacing Your Ingress With the Novel Kubernetes Gateways

This is the repository for the blog post [Replacing Your Ingress With the Novel Kubernetes Gateways](https://benchkram.de/blog/dev/replace-kubernetes-ingress-with-gateway-api).

## Set up the Cluster with Ingress

```bash
kind create cluster --name ingress-example
kubectl config use-context kind-ingress-example
kubectl apply -f nginx-ingress/nginx-ingress-controller.yaml
kubectl apply -f pods-service-manifest.yaml
kubectl apply -f ingress-manifest.yaml

kubectl get pods -n ingress-nginx
kubectl port-forward -n ingress-nginx nginx-ingress-controller-<name> 8080:80
curl localhost:8080/service-a
curl localhost:8080/service-b
```

## Set up the Cluster with Gateways

```bash
kind create cluster --name gateway-example
kubectl config use-context kind-gateway-example
kubectl create namespace nginx-gateway
kubectl create configmap njs-modules --from-file=nginx-gateway/httpmatches.js -n nginx-gateway
kubectl apply -f nginx-gateway/nginx-gateway-controller.yaml
kubectl apply -f gatewayclass-manifest.yaml
# check it's working
kubectl get pods -n nginx-gateway

kubectl apply -f pods-service-manifest.yaml
kubectl apply -f gateway-manifest.yaml

kubectl get pods -n nginx-gateway
kubectl port-forward -n nginx-gateway <name> 8080:80
curl localhost:8080/service-a
curl localhost:8080/service-b
```
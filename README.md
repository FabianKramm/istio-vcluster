# Install Boilerplate
```bash
kind create cluster
istioctl install -f operator.yaml --skip-confirmation
kubectl get crd gateways.gateway.networking.k8s.io &> /dev/null || kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.2.1/standard-install.yaml
```

# Test without vCluster

Install virtual service etc:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f istio.yaml
```

Now test:
```bash
# Exec into client
kubectl exec -it -l app=client -- bash

# Make sure /v2 is available which tells us istio is intercepting the request successfully
curl nginx-service/v2
```



# Test with vCluster 
```bash
kubectl apply -f vcluster-istio.yaml
vcluster create test --upgrade --chart-version 0.24.1 -n vcluster-test
kubectl apply -f deployment.yaml
```

Now test:
```bash
# Exec into client
kubectl exec -it -l app=client -- bash

# Make sure /v2 is available which tells us istio is intercepting the request successfully
curl nginx-service/v2
```

# Install prometheus

Make sure helm is installed to the cluster (see https://github.com/doitintl/eks-basics/tree/master/helm)

## Installation 

```
helm install stable/prometheus-operator --name prometheus-operator --namespace monitoring
```

This will install the prometheus-operator and the grafana service

## Access the grafana dashboard

Expose the grafana dashboard to your machine
```
kubectl port-forward $(kubectl get  pods --selector=app=grafana -n  monitoring --output=jsonpath="{.items..metadata.name}") -n monitoring  3000
```

Access the grafana dashboard over this adderss
```
http://localhost:3000
```

You will see different K8S dashboards added by default.


# Scraping from your service /metrics endpoint

## Example deployment and service with /metrics exposing

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: twelve-clouds
spec:
  selector:
    matchLabels:
      app: twelve-clouds
  replicas: 1 
  template:
    metadata:
      labels:
        app: twelve-clouds 
    spec:
      containers:
      - name: twelve-clouds
        image: karthequian/prom:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: twelve-clouds-service
  labels:
    app: twelve-clouds
spec:
  type: LoadBalancer
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: twelve-clouds
```

## Apply the matching service monitor 
```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
  labels:
    app: prometheus-operator-twelve
    chart: prometheus-operator-6.4.3
    heritage: Tiller
    release: prometheus-operator
  name: prometheus-operator-twelve
  namespace: monitoring
spec:
  endpoints:
  - path: /metrics # REPLACE WITH THE ENDPOINT YOUR APP IS EXPOSING THE METRICS WITH
    port: http # REPLACE WITH THE NAMED PORT OF YOUR SERVICE
  namespaceSelector:
    matchNames:
    - default # REPLACE WITH THE NAMESPACE OF YOUR SERVICE
  selector:
    matchLabels:
      app: twelve-clouds
```
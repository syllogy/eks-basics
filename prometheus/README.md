# Adding prometheus and grafana to the cluster

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


## Example deployment with /metrics exposing

kubectl apply -f https://raw.githubusercontent.com/doitintl/eks-basics/project_init/prometheus/example-deployment.yaml


<details><summary>See the deployment yaml</summary>
<p>
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
```
</p>
</details>


## Expose the service

kubectl apply -f https://raw.githubusercontent.com/doitintl/eks-basics/project_init/prometheus/example-service.yaml

<details><summary>See the service yaml</summary>
<p>
```
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
</p>
</details>

## Apply the matching service monitor 

kubectl apply -f https://raw.githubusercontent.com/doitintl/eks-basics/project_init/prometheus/example-service-monitor.yaml

<details><summary>See the service monitor yaml</summary>
<p>
```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
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
</p>
</details>
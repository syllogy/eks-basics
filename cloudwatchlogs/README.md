# Control plane logging

```
eksctl utils update-cluster-logging --name eksworkshop-eksctl --enable-types all --approve
```

# K8s logging 

add logging role
```
aws iam put-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker --policy-document file://./k8s-logs-policy.json
```


test role
```
aws iam get-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker
```

create namespace

```
kubectl create ns amazon-cloudwatch
```

set params
```
kubectl create configmap cluster-info \
--from-literal=cluster.name=cluster_name \
--from-literal=logs.region=region_name -n amazon-cloudwatch

```
kubectl apply -f fluentd.yml
```


test

```
kubectl get pods -w --namespace=kube-system
```


We are now ready to check that logs are arriving in [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home?#logStream:group=/eks/eksworkshop-eksctl/containers)



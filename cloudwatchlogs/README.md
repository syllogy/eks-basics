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



! Update REGION and CLUSTER_NAME environment variables in fluentd.yml as required. They are set to us-west-2 and eksworkshop-eksctl by default.

```
kubectl apply -f ~/environment/fluentd/fluentd.yml
```


test

```
kubectl get pods -w --namespace=kube-system
```


We are now ready to check that logs are arriving in [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home?#logStream:group=/eks/eksworkshop-eksctl/containers)



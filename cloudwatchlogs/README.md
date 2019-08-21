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


# install helm


## Installation 

### Install helm client to your machine
```
cd ~/environment
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod +x get_helm.sh
./get_helm.sh

```

### Apply tiller Service Account to your cluster

```

kubectl apply -f rbac.yaml

```



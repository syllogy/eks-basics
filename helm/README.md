# Install helm

### Install helm client to your machine
```
cd ~/environment
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod +x get_helm.sh
./get_helm.sh

```

### Apply tiller Service Account to your cluster

```
kubectl apply -f https://raw.githubusercontent.com/doitintl/eks-basics/project_init/helm/rbac.yaml
helm init --service-account tiller

```



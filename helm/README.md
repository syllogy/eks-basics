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
```

<details><summary>See the service account yaml</summary>
<p>

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```
</p>
</details>


### Init helm

```
helm init --service-account tiller
```
### 
The Prometheus Operator should be installed now, Verify it with:

```
kubectl --namespace monitoring get pods -l "release=prometheus-operator"
```

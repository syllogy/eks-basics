# `eks-basics` - Instructions for bootstraping an EKS cluster with cluster backups, logs exporting & monitoring 

## Instal eksctl

To download the latest release, run:

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

<details><summary>eksctl prerequisites</summary>
<p>

You will need to have AWS API credentials configured. What works for AWS CLI or any other tools (kops, Terraform etc), should be sufficient. You can use [`~/.aws/credentials` file][awsconfig]

[awsconfig]: https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html

You will also need [AWS IAM Authenticator for Kubernetes](https://github.com/kubernetes-sigs/aws-iam-authenticator) command (either `aws-iam-authenticator` or `aws eks get-token` (available in version 1.16.156 or greater of AWS CLI) in your `PATH`.

</p>
</details>

## Create a cluster

To create a basic cluster, run:

```
eksctl create cluster
```

<details><summary>More Details</summary>
<p>

A cluster will be created with default parameters
- exciting auto-generated name, e.g. "fabulous-mushroom-1527688624"
- 2x `m5.large` nodes (this instance type suits most common use-cases, and is good value for money)
- use official AWS EKS AMI
- `us-west-2` region
- dedicated VPC (check your quotas)
- using static AMI resolver

Once you have created a cluster, you will find that cluster credentials were added in `~/.kube/config`. If you have `kubectl` v1.10.x as well as `aws-iam-authenticator` commands in your PATH, you should be
able to use `kubectl`. You will need to make sure to use the same AWS API credentials for this also. Check [EKS docs][ekskubectl] for instructions.

[ekskubectl]: https://docs.aws.amazon.com/eks/latest/userguide/configure-kubectl.html

</p>
</details>

## Install Prometheus and Grafana

### Make sure helm is installed

Follow [Installing helm][helmreadme]

[helmreadme]: https://github.com/doitintl/eks-basics/blob/project_init/helm/README.md

### Use helm to deploy Prometheus and Grafana to your cluster

Follow [Adding prometheus and grafana to the cluster][prometheusreadme]

[prometheusreadme]: https://github.com/doitintl/eks-basics/blob/project_init/prometheus/README.md





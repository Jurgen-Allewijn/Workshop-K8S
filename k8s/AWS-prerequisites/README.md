# Prerequisites

## Required for this workshop

You need an AWS Account to execute this workshop. 


> **Warning**
> AWS might charge you a small fee when executing the steps in the workshop (EKS service and 2 m5.large instances)! Clean up steps are included in this demo.

## AWS Cloudshell
All commands exercise are executed into AWS Cloudshell.

### Open AWS Cloudshell

![alt text](/k8s/AWS-prerequisites/aws-cloudshell.png "AWS Cloudshell")

### Install eksctl
To see all possible locations enter the following:

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp && 
sudo mv /tmp/eksctl /usr/local/bin
```

Test that your installation was successful with the following command.
```bash
eksctl version
```

### Start a new EKS cluster

Replace {name of your location} with the location where your current storage account is located.

```bash
eksctl create cluster --name Workshop-K8S
```
> **Note**
> After about ~15 min the cluster should be deployed

###  Verify EKS cluster has started

```bash
kubectl get nodes
```

### Install Helm

```bash
export VERIFY_CHECKSUM=false && \
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash && \
chmod +x ./get_helm.sh && \
./get_helm.sh
```

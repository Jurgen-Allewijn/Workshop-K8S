# Create AWS (EKS) cluster

Follow this guideline to install the nessesery tools and start a EKS cluster.

## Required for this workshop

You need an AWS Account to execute this workshop. 

> **Warning**
> AWS might charge you a small fee when executing the steps in the workshop (EKS service and 2 m5.large instances)! Clean up steps are included in this demo.

## AWS Cloudshell
All commands exercise are executed into AWS Cloudshell.

### Open AWS Cloudshell

![alt text](/k8s/AWS-cluster/aws-cloudshell.png "AWS Cloudshell")

### Install eksctl
eksctl is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes. 

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp && 
sudo mv /tmp/eksctl /usr/local/bin
```

Test that your installation was successful with the following command.
```bash
eksctl version
```

### Install Helm

Helm is a tool that streamlines installing and managing Kubernetes applications and resources.
Click [here](/k8s/helm/) to read more about Helm. 

```bash
export VERIFY_CHECKSUM=false && \
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash && \
chmod +x ./get_helm.sh && \
./get_helm.sh
```

Here is a sample installation output:

```bash
Downloading https://get.helm.sh/helm-v3.5.4-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
````

Verify that helm is working:

```bash
helm version
```

Output

```bash
version.BuildInfo{Version:"v3.5.4", GitCommit:"1b5edb69df3d3a08df77c9902dc17af864ff05d1", GitTreeState:"clean", GoVersion:"go1.15.11"}
````

### Start a new EKS cluster

```bash
eksctl create cluster --name Workshop-K8S
```
> **Note**
> After about ~15 min the cluster should be deployed

###  Verify EKS cluster has started

```bash
kubectl get nodes
```



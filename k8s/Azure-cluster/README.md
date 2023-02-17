# Create Azure (AKS) cluster

AKS is a free container service where nothing will be charged for Kubernetes cluster management. You'll have to pay only for the cloud resources such as VMs, storage, and network resources.

## Required for this workshop

You need an Azure Account with payment Subscription to execute this workshop. 


> **Warning**
> Microsoft will charge you a small fee when executing the steps in the workshop (3 instances of type 
Standard_DS2_v2 used)! Cost will be minimized as we will upgrasde to a free plan. We strongly recomend to clean up the cluster after this demo! Clean up steps are included in this demo.

## Cloud shell
All commands exercise are executed into Azure Cloud shell. Azure Cloud Shell requires an Azure file share to persist files. Create one if requested. Here is walkthrough how to setup  Azure Cloud Shell

### Open cloud shell

![alt text](/k8s/Azure-cluster/azure-portal-cloud-shell.png "Azure Cloud Shell")

### Create a new storage account 
![alt text](/k8s/Azure-cluster/azure-portal-storage-account.png "Azure Cloud Shell")


Let's set a environment variable for the location where we will use to start the resources.

### List available locations
To see all possible locations enter the following:

```bash
az account list-locations -o table
```

### Set default location

Replace {name of your location} with the location where your current storage account is located.

```bash
az configure --defaults location={name of your location}
```

Before we can start with deploying our Azure Kubernetes Cluster we first have to create a resource group:

```bash
az group create --name rg-Workshop-K8S
```

## Install Helm

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


Now that we have the resource group created, we can start with the deploying of the AKS-cluster. 

## Create cluster

```bash
az aks create --resource-group rg-Workshop-K8S --name Workshop-K8S --generate-ssh-keys
````

> **Note**
> After about 5~10 min the cluster should be deployed


### Get credentials to allow you to access the cluster with kubectl
Using the switch --admin or -a admin credentials are added. Default is user credentials

```bash
az aks get-credentials --resource-group rg-Workshop-K8S--name Workshop-K8S
````

###  Verify AKS cluster has started

```bash
kubectl get nodes
```
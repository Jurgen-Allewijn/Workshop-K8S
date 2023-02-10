# What is Azure Kubernetes Service

AKS is a free container service where nothing will be charged for Kubernetes cluster management. You'll have to pay only for the cloud resources such as VMs, storage, and network resources.

# Installing Azure Kubernetes Cluster


Before we can start with deploying our Azure Kubernetes Cluster we first have to create a resource group:

```bash
az group create --name rg-AKS-workshop
```

Now that we have the resource group created, we can start with the deploying of the AKS-cluster. We will deploy a cluster with three nodes. The control plane will not be visible to us as this part of the service offered by Microsoft. We will do the deployment in two steps1

# Step 1 Create cluster

```bash
az aks create --resource-group rg-AKS-workshop --name AKS-Workshop --generate-ssh-keys 
````

> **Note**
> After about 5~10 min the cluster should be deployed

# Step 2 Update cluster

```bash
az aks update --resource-group rg-AKS-workshop --name AKS-Workshop --no-uptime-sla
````

> **Note**
> After this command has completed, the free version of AKS is used!

# Get credentials to allow you to access the cluster with kubectl

```bash
az aks get-credentials --resource-group rg-AKS-workshop --name AKS-Workshop
````




List all your nodes in the cluster:

```bash
$ kubectl get nodes
```
We can see the pods running on the master: etcd, api-server, controller manager and scheduler, as well as etcd and Weave pods.

We now have a Kubernetes cluster with one master and two workers

TIP! for ease of use set autocompletion and alias for kubectl

```bash
$ source <(kubectl completion bash)
$ echo "source <(kubectl completion bash)" >> ~/.bashrc
$ alias k=kubectl
$ complete -F __start_kubectl k
```

Now you can use the command `k` instead of `kubectl`

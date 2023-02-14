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


# Get admin credentials to allow you to access the cluster with kubectl
Default is user credentials

```bash
az aks get-credentials --resource-group rg-AKS-workshop --name AKS-Workshop --admin
````


List all your nodes in the cluster:

```bash
kubectl get nodes -o wide
```

We now have a Kubernetes cluster three nodes. The control plane is not visible as it is managed by Microsoft. Adding the -o wide at the end of the command gives  more info about the nodes like ip adress, kukbernetes version, etc.

TIP! for ease of use set autocompletion and alias for kubectl

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
alias k=kubectl
complete -F __start_kubectl k
```

Now you can use the command `k` instead of `kubectl`



# Useful commands

Get config
```
kubectl config view
```

List nodes
```
kubectl get nodes -o wide
```

List pods
```
kubectl get pods -A
```

List namespces

```
kubectl get ns
```



List deployments

```
kubectl get deployments -A
```
>Add -w  after get to watch (tail) the view

List deployments | matching STRING

```
kubectl get deployments -A | egrep <STRING> 
```

Explain resource

```
kubectl explain <RESOURCE>
```
> Possible resource types include: pods (po), services (svc), replicationcontrollers (rc), nodes (no), events (ev), componentstatuses (cs), limitranges (limits), persistentvolumes (pv), persistentvolumeclaims (pvc), resourcequotas (quota), namespaces (ns), horizontalpodautoscalers (hpa) or endpoints (ep).

Get RESOURCE performence info 

```
kubectl top <RESOURCE>
```

Scale deployment

```
kubectl scale --replicas=n deployments/<NAME> -n <NAMESPACE>
```

List all nodes, with more details

```
kubectl get nodes -o wide
```

List all namespces

```
kubectl get ns
```

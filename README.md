# Kubernetes workshop

This project contains labs for the kubernetes workshop. In this workshop we will deploy a kubernetes cluster. We look at deployment strategies and take a first glance at observability.


- [Prerequisites](k8s/prerequisites/)


## Clone Git Repository

For we proceed with the excercises, let's first clone the repository, so all the files are locally available.

```bash
git clone https://github.com/Jurgen-Allewijn/Workshop-K8S.git
````

## Create Kubernetes Cluster

In this workshop we are using Azure Kubernetes Service (AKS). 

- [Create AKS cluster](k8s/k8s-cluster/)

## Helm

- [Helm](k8s/helm/)

## NGINX Ingress Controller

- [NGINX Ingress Controller](k8s/ingress/)


## Deployment Strategies

- [Rolling Update](k8s/rolling-update/)
- [Recreate](k8s/recreate/)
- [Blue Green](k8s/blue-green/)
- [Canary](k8s/canary/)
- [A/B Testing](k8s/a-b-testing/)

## Observabilty

- [Logging](k8s/logging/)
- [Monitoring](k8s/monitoring/)

##  Clean up

### 1. Removing the AKS Cluster

```bash
az aks delete --name AKS-Workshop --resource-group rg-AKS-workshop -y 
```
> Note: It might take 3-6 minutes to delete the cluster


### 2. Removing the Azure Resource Group

```bash
az group delete --resource-group rg-AKS-Workshop -y
```

### 3. Removing the AKS Kubeconfig Entries

```bash
kubectl config delete-cluster AKS-Workshop
kubectl config delete-context AKS-Workshop
kubectl config delete-user clusterUser_rg-AKS-workshop_AKS-Workshop
```

### 4. Deleting the Cloud Shell Instance

```bash
clouddrive unmount
```
> You will be prompted to confirm twice.

>WARN: Removing a file share from Cloud Shell will terminate your current session.
Do you want to continue(y/n): y

> WARN: You will be prompted to create and mount a new file share on your next session.
Do you want to continue(y/n): y

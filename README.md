# Kubernetes workshop

This project contains labs for the kubernetes workshop.

# In this workshop we will use the application:

#- <https://github.com/avthart/hello-app>

## Prerequisites

## Required for this workshop

You need an Azure Account with payment Subscription to execute this workshop. 


> **Warning**
> Microsoft will charge you a small fee when executing this demo (3 instances of type 
Standard_DS2_v2 used)! We strongly recomend to clean up the cluster after this demo! Clean up steps are included in this demo.

## Cloud shell
All commands exercise are executed into Azure Cloud shell. Azure Cloud Shell requires an Azure file share to persist files. Create one if requested. Here is walkthrough how to setup  Azure Cloud Shell

### Open cloud shell
![alt text](./azure-portal-cloud-shell.png "Azure Cloud Shell")

### Create a new storage account 
![alt text](./azure-portal-storage-account.png "Azure Cloud Shell")


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

- [Prerequisites](k8s/prerequisites/)

## Create Kubernetes Cluster

In this workshop we are using Azure Kubernetes Service (AKS). 

- [Create AKS cluster](k8s/k8s-cluster/)

## Helm

- [Helm](k8s/helm/)

## NGINX Ingress Controller

- [NGINX Ingress Controller](k8s/ingress/)

## Clone Git Repository

- [Clone Git Repository](k8s/git/)

## Deployment Strategies

- [Rolling Update](k8s/rolling-update/)
- [Recreate](k8s/recreate/)
- [Blue Green](k8s/blue-green/)
- [Canary](k8s/canary/)
- [A/B Testing](k8s/a-b-testing/)

## Observabilty

- [Logging](k8s/logging/)
- [Monitoring](k8s/monitoring/)

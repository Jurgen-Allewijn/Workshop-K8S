# Prerequisites

## Required for this workshop

You need an Azure Account with payment Subscription to execute this workshop. 


> **Warning**
> Microsoft will charge you a small fee when executing the steps in the workshop (3 instances of type 
Standard_DS2_v2 used)! Cost will be minimized as we will upgrasde to a free plan. We strongly recomend to clean up the cluster after this demo! Clean up steps are included in this demo.

## Cloud shell
All commands exercise are executed into Azure Cloud shell. Azure Cloud Shell requires an Azure file share to persist files. Create one if requested. Here is walkthrough how to setup  Azure Cloud Shell

### Open cloud shell

![alt text](/k8s/prerequisites/azure-portal-cloud-shell.png "Azure Cloud Shell")

### Create a new storage account 
![alt text](/k8s/prerequisites/azure-portal-storage-account.png "Azure Cloud Shell")


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

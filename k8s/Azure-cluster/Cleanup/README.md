#  Clean up

## 1. Removing the AKS Cluster

```bash
az aks delete --name Workshop-K8S --resource-group rg-Workshop-K8S -y 
```
> Note: It might take 3-6 minutes to delete the cluster


## 2. Removing the Azure Resource Group

```bash
az group delete --resource-group rg-Workshop-K8S -y
```

## 3. Removing the AKS Kubeconfig Entries

```bash
kubectl config delete-cluster Workshop-K8S
kubectl config delete-context Workshop-K8S
kubectl config delete-user clusterUser_rg-Workshop-K8S_Workshop-K8S
```

## 4. Deleting the Cloud Shell Instance

```bash
clouddrive unmount
```
> You will be prompted to confirm twice.

>WARN: Removing a file share from Cloud Shell will terminate your current session.
Do you want to continue(y/n): y

> WARN: You will be prompted to create and mount a new file share on your next session.
Do you want to continue(y/n): y
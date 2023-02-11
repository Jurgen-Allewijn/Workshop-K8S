# Kubernetes workshop

This project contains labs for the kubernetes workshop. In this workshop we will deploy a kubernetes cluster. We look at deployment strategies and take a first glance at observability.

- [Prerequisites](k8s/prerequisites/)

## Create Kubernetes Cluster

In this workshop we are using Azure Kubernetes Service (AKS). 

- [Create AKS cluster](k8s/k8s-cluster/)

## Helm

- [Helm](k8s/helm/)

## NGINX Ingress Controller

- [NGINX Ingress Controller](k8s/ingress/)

## Clone Git Repository

For we proceed with the excercises, let's first clone the repository, so all the files are locally available.

```bash
git clone https://github.com/Jurgen-Allewijn/Workshop-K8S.git
````

## Deployment Strategies

- [Rolling Update](k8s/rolling-update/)
- [Recreate](k8s/recreate/)
- [Blue Green](k8s/blue-green/)
- [Canary](k8s/canary/)
- [A/B Testing](k8s/a-b-testing/)

## Observabilty

- [Logging](k8s/logging/)
- [Monitoring](k8s/monitoring/)

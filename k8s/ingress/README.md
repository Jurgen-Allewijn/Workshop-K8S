# Ingress

Ingress is the built‑in Kubernetes load‑balancing framework for HTTP traffic. With Ingress, you control the routing of external traffic. When running on public clouds like AWS or GKE the load-balancing feature is available out of the box.

**Why Ingress?**
For each service with LoadBalancer type, AWS/GCP/Azure will create a new ELB (which comes with costs if you have a lot of services). With Kubernetes ingress you will need only one for one IP address. There are several Ingress controllers like NGINX/NGINX Plus, Traefik, Voyager (HAProxy) or Contour (Envoy) but also Amazon and Google offer Ingress implementations (AWS Aplication Load Balancer or Google Cloud Load Balancer)

Ingress is the most useful if you want to expose multiple services under the same IP address, and these services all use the same L7 protocol (typically HTTP). You can get a lot of features out of the box (like SSL, Auth, Routing, etc) depending on the ingress implementation.

Read more about Load Balancer and Ingress Controllers here:
https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0

We will use the Nginx ingress controller which is probably the most used at the moment.
https://kubernetes.github.io/ingress-nginx/

## Prerequisites

Before installing NGINX, create a namespace for your ingress resources.

```bash
kubectl create namespace ingress-nginx
```

### Add the ingress-nginx helm repository

NGINX Ingress controller can be installed via Helm using the chart from the project repository. To install the chart with the release name **ingress-nginx**

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
````
Output:
```bash
"ingress-nginx" has been added to your repositories
````

```bash
helm repo update
```

## Installation

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
```

When the Kubernetes load balancer service is created for the NGINX ingress controller, normally it waits for dynamic public IP address to be assigned, as shown in the following example output:

```bash
kubectl --namespace  ingress-nginx get services -o wide -w ingress-nginx-controller
`````
Example ouput

```bash
NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE   SELECTOR
ingress-nginx-controller   LoadBalancer   10.111.45.151   <pending>     80:30011/TCP,443:30740/TCP   41s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
````

Confirm the installation:

```bash
helm list --all-namespaces
````

Example output:

```bash
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
ingress-nginx   ingress-nginx   1               2021-05-19 15:02:26.127627562 +0000 UTC deployed        ingress-nginx-3.31.0    0.46.0
````

Check if the ingress controller is listening on port 80 and 443

Deploy a demo app

```bash
cd ~/Workshop-K8S/k8s/ingress
kubectl apply -f app.yml
````

Validate if the application and services are running

```bash
kubectl get pods,svc -A
`````



Go to your browser and connect to http://[External ip adress load balancer] and https://[External ip adress load balancer]

## Optional excercises



##  Azure Voting App

This sample creates a multi-container application in an Azure Kubernetes Service (AKS) cluster. The application interface has been built using Python / Flask. The data component is using Redis. More information/ backgroud of this app can be found here https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli

First we deploy the application using the kubectl command.

```bash
kubectl apply -f azure-vote.yaml
````

Output:
```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
````

It can take some time to deploy everything. To check if the deployment is ready for use, use the --watch  or -w switch

```bash
kubectl get service azure-vote-front --watch
````

The EXTERNAL-IP output for the azure-vote-front service will initially show as pending.

```bash
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
````

Once the EXTERNAL-IP address changes from pending to an actual public IP address, use CTRL-C to stop the kubectl watch process. The following example output shows a valid public IP address assigned to the service:

```bash
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
````

To check if the application is working open a web browser to the external IP address of your service.

![alt text](/k8s/ingress/azure-voting-application.png)
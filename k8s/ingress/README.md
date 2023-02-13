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

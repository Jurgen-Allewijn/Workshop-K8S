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
$ kubectl create namespace ingress-nginx
```

### Add the ingress-nginx helm repository

NGINX Ingress controller can be installed via Helm using the chart from the project repository. To install the chart with the release name **ingress-nginx**

```bash
$ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

"ingress-nginx" has been added to your repositories

$ helm repo update
```

## Installation

```bash
$ helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
```

When the Kubernetes load balancer service is created for the NGINX ingress controller, normally it waits for dynamic public IP address to be assigned, as shown in the following example output:

```bash
$ kubectl --namespace  ingress-nginx get services -o wide -w ingress-nginx-controller

NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE   SELECTOR
ingress-nginx-controller   LoadBalancer   10.111.45.151   <pending>     80:30011/TCP,443:30740/TCP   41s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
```

Confirm the installation:

```bash
$ helm list --all-namespaces

NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
ingress-nginx   ingress-nginx   1               2021-05-19 15:02:26.127627562 +0000 UTC deployed        ingress-nginx-3.31.0    0.46.0
```

## Configuration

The service type in the Helm chart is based on a external Loadbalancer. In our environment there is no external Loadbalancer configured which exposes the service externally using a cloud provider's loadbalancer, such as for example Azure Loadbalancer. When configuring it on-premise you can make use of [MetalLB](https://metallb.universe.tf/), which is a loadbalancer implementation for bare metal.

Change the port numbers of the HTTP to 30080 & HTTPS to 30443 and set the `type: LoadBalancer` in the yaml to `type: NodePort`.

```bash
$ kubectl edit service -n ingress-nginx ingress-nginx-controller
```

The spec configuration of the service should look like this:

```yaml

---
ports:
  - name: http
    nodePort: 30080
    port: 80
    protocol: TCP
    targetPort: http
  - name: https
    nodePort: 30443
    port: 443
    protocol: TCP
    targetPort: https
selector:
  app.kubernetes.io/component: controller
  app.kubernetes.io/instance: ingress-nginx
  app.kubernetes.io/name: ingress-nginx
sessionAffinity: None
type: NodePort
```

## Test

Test if you can reach the HTTP & HTTP endpoint with your browser. No ingress rules have been created yet, so the NGINX ingress controller's default 404 page is displayed.

```
http://kb-student<NUMBER>-node<NUMBER>.westeurope.cloudapp.azure.com:30080
https://kb-student<NUMBER>-node<NUMBER>.westeurope.cloudapp.azure.com::30443
```

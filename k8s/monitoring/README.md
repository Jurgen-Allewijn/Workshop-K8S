# Monitoring

This workshop will show you how to install Prometheus and Grafana for scraping the metrics of the NGINX Ingress controller.

The controller should be configured for exporting metrics. This requires 3 configurations to the controller. These configurations are :

- controller.metrics.enabled=true
- controller.podAnnotations."prometheus.io/scrape"="true"
- controller.podAnnotations."prometheus.io/port"="10254"

The easiest way to configure the controller for metrics is via helm upgrade. Assuming you have installed the ingress-nginx controller as a helm release named ingress-nginx, then you can simply type the command show below :

```bash
helm upgrade ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx \
    --set controller.metrics.enabled=true \
    --set-string controller.podAnnotations."prometheus\.io/scrape"="true" \
    --set-string controller.podAnnotations."prometheus\.io/port"="10254"
````

You can validate that the controller is configured for metrics by looking at the values of the installed release, like this:

```bash
helm get values ingress-nginx --namespace ingress-nginx
````

You should be able to see the values shown below:

```yaml
USER-SUPPLIED VALUES:
controller:
  metrics:
    enabled: true
  podAnnotations:
    prometheus.io/port: "10254"
    prometheus.io/scrape: "true"
````

## Deploy and configure Prometheus Server

The Prometheus server must be configured so that it can discover endpoints of services.
The rest of this tutorial will guide you through the steps needed to deploy a properly configured Prometheus server.

Running the following command deploys prometheus in Kubernetes:

```bash
kubectl apply --kustomize github.com/kubernetes/ingress-nginx/deploy/prometheus/
````

The ouput will be:

```bash
serviceaccount/prometheus-server created
role.rbac.authorization.k8s.io/prometheus-server created
rolebinding.rbac.authorization.k8s.io/prometheus-server created
configmap/prometheus-configuration-8hk4m6bf76 created
service/prometheus-server created
deployment.apps/prometheus-server created
````

Take a look at the services

```bash
kubectl get svc -n ingress-nginx
````

```log
NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.99.168.46     <IP or URI>   80:30080/TCP,443:30443/TCP   15m
ingress-nginx-controller-admission   ClusterIP      10.100.247.181   <none>        443/TCP                      15m
ingress-nginx-controller-metrics     ClusterIP      10.97.209.222    <none>        10254/TCP                    7m48s
prometheus-server                    LoadBalancer   10.106.249.223   <IP or URI>   80:32563/TCP                 78s
```


```bash
kubectl edit service -n ingress-nginx prometheus-server
````

Configure the port to `port: 80`

The part of the spec configuration of the service should look like this:

```yaml
---
ports:
  - nodePort: 32563
    port: 80
    protocol: TCP
    targetPort: 9090
selector:
  app.kubernetes.io/name: prometheus
  app.kubernetes.io/part-of: ingress-nginx
sessionAffinity: None
type: LoadBalancer
````

You should see a change in the port configuration:

```bash
kubectl get svc -n ingress-nginx prometheus-server
````
Exsample
```log
NAME                TYPE            CLUSTER-IP       EXTERNAL-IP        PORT(S)          AGE
prometheus-server   Loadbalancer    10.106.249.223   <IP or URI>        80:31413/TCP     10m
````

Open your browser and visit the following URL to load the Prometheus Dashboard: 

```
http://<IP or URI>
```

## Deploy and configure Grafana

Install grafana using the below command:

```bash
kubectl apply --kustomize github.com/kubernetes/ingress-nginx/deploy/grafana/
````

Ouput:

```bash
service/grafana created
deployment.apps/grafana created
```

Take a look at the services

```bash
kubectl get svc -n ingress-nginx
````

```log
NAME                                 TYPE             CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
grafana                              LoadBalancer     10.110.154.157   <IP or URI>   80:31423/TCP                 11s
ingress-nginx-controller             LoadBalancer     10.99.168.46     <IP or URI>   80:30080/TCP,443:30443/TCP   29m
ingress-nginx-controller-admission   ClusterIP        10.100.247.181   <none>        443/TCP                      29m
ingress-nginx-controller-metrics     ClusterIP        10.97.209.222    <none>        10254/TCP                    22m
prometheus-server                    LoadBalancer     10.106.249.223   <IP or URI>   80:32563/TCP                 15m
```

Configure the port to `port: 80`

```bash
kubectl edit service -n ingress-nginx grafana
```

The part of the spec configuration of the service should look like this:

```yaml
---
ports:
  - nodePort: 31423
    port: 80
    protocol: TCP
    targetPort: 3000
selector:
  app.kubernetes.io/name: grafana
  app.kubernetes.io/part-of: ingress-nginx
sessionAffinity: None
type: LoadBalancer
```

You should see a change in the port configuration:

```bash
kubectl get svc -n ingress-nginx grafana
```

```log
NAME      TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)          AGE
grafana   LoadBalancer   10.110.154.157   <IP or URI>       80:31655/TCP     2m15s
```

Open your browser and visit the following URL to load the Grafana Dashboard: 

```
http://<IP or URI>
```

The default credentials are: `admin:admin`

After the login you can import the Grafana dashboard from official dashboards, by following steps given below:

```txt
Navigate to lefthand panel of grafana
Hover on the gearwheel icon for Configuration and click "Data Sources"
Click "Add data source"
Select "Prometheus"
Enter the details (note: http://<CLUSTER-IP_OF_PROMETHEUS_SERVICE>)
Left menu (hover over +) -> Dashboard -> Click "Import"
Enter the copy pasted json from https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/grafana/dashboards/nginx.json
Click Import JSON
Select the Prometheus data source
Click "Import"
````

Now you should see a simpel dashboard.

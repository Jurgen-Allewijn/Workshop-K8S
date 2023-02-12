# Rolling Update

A ramped deployment updates pods in a rolling update fashion, a secondary ReplicaSet is created with the new version of the application, then the number of replicas of the old version is decreased and the new version is increased until the correct number of replicas is reached.

![rolling-update](rollingupdate.gif "Rolling Update")

## Create namespace

```bash
kubectl create namespace rolling-update
````

## Deploy App

```bash
cd ~/kubernetes-workshop/k8s/rolling-update
````

```bash
kubectl -n rolling-update apply -f hello-world.yaml
````

```bash
kubectl -n rolling-update rollout status deployment hello-world -w
````

```bash
kubectl -n rolling-update get events
````

```bash
kubectl -n rolling-update get pods -o wide
````

## Create Service (ClusterIP, NodePort, LoadBalancer)

```bash
kubectl -n rolling-update apply -f hello-world-service.yaml
````

```bash
kubectl -n rolling-update get service
```

## Create Ingress

```bash
kubectl -n rolling-update apply -f hello-world-ingress.yaml
```

## Test

Put the Public IP adres of node0 in a enviroment variable, don't forget to change the student number.

```bash
export IP_ADDRESS="$(nslookup kb-student<NUMBER>-node0.westeurope.cloudapp.azure.com | awk '/^Address:/ {A=$2}; END {print A}')"
echo $IP_ADDRESS
````

Test with curl:

```bash
url http://$IP_ADDRESS:30080/api
```

Or in browser

http://[PUBLIC IP]:30080

# Self healing

Find the right pod name in the rolling-update namespace and delete the pod:

```bash
kubectl -n rolling-update get pods
kubectl -n rolling-update delete pod <hello-world pod name>
````

See what happened and if a new pod is started:

```bash
kubectl -n rolling-update get events
````

```bash
kubectl -n rolling-update get pods
````

# Healtcheck

Check health check:

```bash
curl $IP_ADDRESS:30080/health
````

Marks server as down:

```bash
curl -X POST $IP_ADDRESS:30080/down
````

Check health check:

```bash
curl $IP_ADDRESS:30080/health
```

Check that health check is failed:

```bash
kubectl -n rolling-update get events -w
````

```bash
kubectl -n rolling-update get pods -o wide
````

# Update & Rollback

Update App to use new version

```bash
kubectl -n rolling-update set image --record deployments/hello-world hello-world=avthart/hello-app:1.0.1
````

This results in a new pod with status broken version (status CrashLoopBackOff). The new version is broken :-(

Roll out status

```bash
kubectl -n rolling-update rollout status deployments/hello-world
````

```bash
kubectl -n rolling-update get pods
````

```bash
kubectl -n rolling-update describe pod hello-world
````

Which version is running? Stil alive?

```bash
curl $IP_ADDRESS:30080/api
```

Rollback App

```bash
kubectl -n rolling-update rollout history deployments/hello-world
````

```bash
kubectl -n rolling-update rollout history deployments/hello-world --revision=2
````

```bash
kubectl -n rolling-update rollout undo deployments/hello-world
````

Update to working version:

```bash
kubectl -n rolling-update set image --record deployments/hello-world hello-world=avthart/hello-app:1.0.3
````

```bash
kubectl -n rolling-update rollout status deployments/hello-world
````

Which version is running? Was there any downtime?

```bash
curl $IP_ADDRESS:30080/api
````

# Scaling

Scale app to 3 instances

```bash
kubectl -n rolling-update scale --replicas=3 deployment/hello-world
````

```bash
kubectl -n rolling-update rollout status deployments/hello-world
````

```bash
kubectl -n rolling-update get pods
````

Check that request is handled by all instances:

```bash
while sleep 0.5; do curl $IP_ADDRESS:30080/api; done
````

Stop with CONTROL-C

# Clean up

```bash
kubectl delete ns rolling-update
````

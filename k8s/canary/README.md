# Canary

A canary deployment consists of routing a subset of users to a new functionality. In Kubernetes, a canary deployment can be done using two Deployments with common pod labels. One replica of the new version is released alongside the old version. Then after some time and if no error is detected, scale up the number of replicas of the new version and delete the old deployment.

![canary](canary.gif "Canary")

## Go to the right directory

```bash
cd ~/kubernetes-workshop/k8s/canary/
````

## Create namespace

```bash
kubectl create namespace canary
````

## Deploy v1

```bash
kubectl -n canary apply -f hello-world-v1.yaml
````

## Test

Test:

```bash
curl http://$IP_ADDRESS:30080/api
````

Or in browser:
http://[PUBLIC IP_ADDRESS]:30080

## Deploy v2 besides v1

```bash
kubectl -n canary apply -f hello-world-v2.yaml
````

Check that requests are handled by v1 and v2

```bash
while sleep 0.5; do curl $IP_ADDRESS:30080/api; done
````

Stop with CONTROL-C

# Scale v2 to 3

```bash
kubectl -n canary scale --replicas=3 deployment/hello-world-v2
````

```bash
kubectl -n canary get pods -w
````

Test:

```bash
curl http://$IP_ADDRESS:30080/api
````

Or in browser:
http://[PUBLIC IP ADDRESS]:30080

# Delete v1

```bash
kubectl -n canary delete deployment/hello-world-v1
````

```bash
kubectl -n canary get pods
````

Test:

```bash
curl http://$IP_ADDRESS:30080/api
````

Or in browser:
http://[PUBLIC IP ADDRESS]:30080

# Clean up

```bash
kubectl delete ns canary
````

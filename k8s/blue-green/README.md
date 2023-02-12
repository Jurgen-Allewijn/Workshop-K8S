# Blue Green

A blue/green deployment differs from a ramped deployment because the “green” version of the application is deployed alongside the “blue” version. After testing that the new version meets the requirements, we update the Kubernetes Service object that plays the role of load balancer to send traffic to the new version by replacing the version label in the selector field.

![bluegreen](blue-green.gif "Blue-Green")

## Go to the right diorectory

```bash
cd ~/kubernetes-workshop/k8s/blue-green/
````

## Create namespace

```bash
kubectl create ns blue-green
````

## Deploy Blue v1

```bash
kubectl -n blue-green apply -f hello-world-blue.yaml
````

```bash
kubectl -n blue-green get pods
````

## Test

With CURL:

```bash
curl $IP_ADDRESS:30080/api
````

And in browser you should see the blue version (1.0.0):

<http://[PUBLIC IP_ADDRESS>]:30080

## Deploy green v2

```bash
kubectl -n blue-green apply -f hello-world-green.yaml
````

```bash
kubectl -n blue-green get pods -w
````

Stop with CONTROL-C

## Test

With CURL:

```bash
curl $IP_ADDRESS:30080/api
````

And in browser you should see the blue version (1.0.0):

http://[PUBLIC IP_ADDRESS]:30080

## Switch to green

Check that service is pointing to `blue` version:

```bash
kubectl -n blue-green describe svc hello-world-service
````

Change to `green` version:

```bash
kubectl -n blue-green patch service hello-world-service -p '{"spec":{"selector":{"app":"hello-world-green"}}}'
````

## Test

With CURL:

```bash
curl $IP_ADDRESS:30080/api
````

And in browser you should see the green version (2.0.0):

http://[PUBLIC IP_ADDRESS]:30080

# Clean up

```bash
kubectl delete ns blue-green
````

# Logging within kubernetes

This exercise creates a pod on the Kubernetes cluster which logs the timestamp.
With kubectl the logs can be viewed.

### Goto the git workshop logging directory:

```bash
cd /Workshop-K8S/k8s/logging/
````

### view the content of counter-pod.yaml

```bash
cat counter-pod.yaml
````

The output is:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: counter
spec:
  containers:
    - name: count
      image: busybox
      args:
        [
          /bin/sh,
          -c,
          'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done',
        ]
```

### Create log container in default namespace

```bash
kubectl apply -f counter-pod.yaml
````

### The output is:

```bash
pod/counter created
````

### Look at the logs of this pod

```bash
kubectl logs counter
````

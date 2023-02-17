# Useful commands

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
alias k=kubectl
complete -F __start_kubectl k
```

Now you can use the command `k` instead of `kubectl`

Get config
```
kubectl config view
```

List nodes
```
kubectl get nodes -o wide
```

List pods
```
kubectl get pods -A
```

List namespces

```
kubectl get ns
```

List deployments

```
kubectl get deployments -A
```
>Add -w  after get to watch (tail) the view

List deployments | matching STRING

```
kubectl get deployments -A | egrep <STRING> 
```

Explain resource

```
kubectl explain <RESOURCE>
```
> Possible resource types include: pods (po), services (svc), replicationcontrollers (rc), nodes (no), events (ev), componentstatuses (cs), limitranges (limits), persistentvolumes (pv), persistentvolumeclaims (pvc), resourcequotas (quota), namespaces (ns), horizontalpodautoscalers (hpa) or endpoints (ep).

Get RESOURCE performence info 

```
kubectl top <RESOURCE>
```

Scale deployment

```
kubectl scale --replicas=n deployments/<NAME> -n <NAMESPACE>
```

List all nodes, with more details

```
kubectl get nodes -o wide
```

List all namespces

```
kubectl get ns
```

# Alternative

## (Optional) When there are errors while joining we could setup the master node to allow scheduling of pods

```bash
$ kubectl get nodes
$ kubectl taint node <NAMEOFMASTER> node-role.kubernetes.io/master:NoSchedule-
```

Verify the master isn't tainted

```bash
$ kubectl describe node | egrep -i taint
```

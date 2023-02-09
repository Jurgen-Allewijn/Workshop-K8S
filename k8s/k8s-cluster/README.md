# Installing Kubernetes Cluster

Set up a Kubernetes cluster with one master and two nodes using Kubeadm
Make sure you have the details of your three nodes

## Creating a cluster with kubeadm (On node0 (Control-plane / Master))

To initialize the control-plane node run:

```bash
$ sudo kubeadm init
```

If successful, the output will end with a join command:

```bash
...
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.0.0.4:6443 --token 11du93.hdevwouym6hakgmx \
        --discovery-token-ca-cert-hash sha256:90bda067485618a65d1263d9d36adf564f456fc54644089b6bce3b70d2577a81
```

Please copy the "kubeadm join" output to a notepad for later use

To start using you cluster, you need to run the following as a regular user commands on node0:

```bash
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

List all your nodes in the cluster:

```bash
$ kubectl get nodes
```

Which should output something like:

```bash
NAME   STATUS   ROLES                 AGE   VERSION
node0  NotReady control-plane,master  33m   v1.22.2
```

The NotReady status indicates that we must install a network for our cluster.

## Installing a Pod network add-on (On node0 (Control-plane / Master))

Let's install Weave Net network driver (CNI):

```bash
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

```bash
$ watch kubectl get pods -A
```

You should see that a number of new objects/services have been created.

## Joining a cluster with kubeadm (On node1 & node2 (workers))

Execute the join command you found above when initializing Kubernetes on node1 and node2, you'll need to add sudo to the start.

```bash
$ sudo kubeadm join ...
```

If there are preflight errors then use:

```bash
$ sudo kubeadm join ... --ignore-preflight-errors=SystemVerification
```

When by accident you've lost the join command, retrieve it by:

```bash
$ sudo kubeadm token create --print-join-command
```

### On node0 (master)

Check the status back on node0:

```bash
watch kubectl get nodes
```

After a few moments, there should be three nodes listed - all with the Ready status.

Set node worker role on node1 and node2

```bash
$ kubectl get nodes
$ kubectl label nodes node1 kubernetes.io/role=worker
$ kubectl label nodes node2 kubernetes.io/role=worker
$ kubectl get nodes
```

Let's see what system pods are running on our cluster:

```bash
$ kubectl get pods -n kube-system
$ kubectl get pods -A
```

which results in something similar to this:

```bash
NAMESPACE     NAME                            READY   STATUS    RESTARTS       AGE
kube-system   coredns-78fcd69978-5vnx6        1/1     Running   0              3m42s
kube-system   coredns-78fcd69978-bbpt9        1/1     Running   0              3m42s
kube-system   etcd-node0                      1/1     Running   0              3m58s
kube-system   kube-apiserver-node0            1/1     Running   0              3m56s
kube-system   kube-controller-manager-node0   1/1     Running   0              3m56s
kube-system   kube-proxy-nmcsr                1/1     Running   0              3m43s
kube-system   kube-proxy-zhh6h                1/1     Running   0              50s
kube-system   kube-scheduler-node0            1/1     Running   0              3m56s
kube-system   weave-net-gl6rm                 2/2     Running   1 (3m6s ago)   3m16s
kube-system   weave-net-r2zf4                 2/2     Running   1 (3s ago)     50s
kube-system   weave-net-rxrl4                 2/2     Running   1 (3s ago)     50s
```

We can see the pods running on the master: etcd, api-server, controller manager and scheduler, as well as etcd and Weave pods.

We now have a Kubernetes cluster with one master and two workers

TIP! for ease of use set autocompletion and alias for kubectl

```bash
$ source <(kubectl completion bash)
$ echo "source <(kubectl completion bash)" >> ~/.bashrc
$ alias k=kubectl
$ complete -F __start_kubectl k
```

Now you can use the command `k` instead of `kubectl`

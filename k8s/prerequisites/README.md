# Prerequisites

Repeat these steps 3 times, each for every node.

## Installing Container Runtime Interface (CRI)

Installation methods

You can install Docker Engine in different ways, depending on your needs:

Option 1: In testing and development environments, some users choose to use automated convenience scripts to install Docker.

Option 2 - recommended: Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks.

### (Option 1) Install using the convenience script

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

### (Option 2 - recommended) Install using the repository

Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```bash
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Add Docker’s official GPG key:

```bash
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Set the stable repository

```bash
$ sudo echo \
    "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install Docker Engine

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### Test and configure docker

Verify that Docker Engine is installed correctly by running the hello-world image.

```bash
$ sudo docker run hello-world
```

Use Docker as a non-root user
We don‘t want docker commands to be run with sudo privileges only. Instead we want the current user to run the docker command. In order to do this, we need to add the user to the docker group.

```bash
$ sudo usermod -aG docker ubuntu
```

Now, you need to log out and log in so that affect of usermod will take place.

```bash
$ logout
```

Configure Docker to start on boot

```bash
$ sudo systemctl enable docker
```

### Set systemd cgroup driver (as root user)

```bash
$ sudo -i

whoami

cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

systemctl daemon-reload
systemctl restart docker

logout
```

## Installing kubeadm

Update the apt package index and install packages needed to use the Kubernetes apt repository:

```bash
$ sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
```

Download the Google Cloud public signing key:

```bash
$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

```

Add the Kubernetes apt repository:

```bash
$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:

```bash
$ sudo apt-get update
$ sudo apt-get install -y kubelet kubeadm kubectl
$ sudo apt-mark hold kubelet kubeadm kubectl
```

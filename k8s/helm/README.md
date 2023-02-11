# Helm

Helm is a tool that streamlines installing and managing Kubernetes applications and resources. Think of it like apt/yum/homebrew for Kubernetes. Use of Helm charts is recommended, because they are maintained and typically kept up to date by the Kubernetes community.

In this workshop we are installing Helm 3 instead of Helm 2 for security reasons. Helm 3 doesn’t have the server/client architecture like Helm 2. There is no tiller server component. So the installation is just for the helm command line component which interacts with Kubernetes through your kubectl configuration file and the default Kubernetes RBAC.

## Install Helm 3

In our case we will install helm on the master node node-0 using the following commands:

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Here is a sample installation output:

```bash
Downloading https://get.helm.sh/helm-v3.5.4-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
````

## Confirm the installation of Helm 3

Verify that helm is working:

```bash
helm version
`````

Output

```bash
version.BuildInfo{Version:"v3.5.4", GitCommit:"1b5edb69df3d3a08df77c9902dc17af864ff05d1", GitTreeState:"clean", GoVersion:"go1.15.11"}
````

## Working with Repositories

Once you have Helm ready, you can add a chart repository. Helm 3 no longer ships with a default chart repository. Therefore, check [Artifact Hub](https://artifacthub.io/) for available Helm chart repositories.

The helm repo command group provides commands to add, list, and remove repositories. New repositories can be added with helm repo add:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
````
Ouput: 

```bash
"bitnami" has been added to your repositories
```

Because chart repositories change frequently, at any point you can make sure your Helm client is up to date by running helm repo update.

You can see which repositories are configured using helm repo list:

```bash
helm repo list
```

Repositories can be removed with helm repo remove.

## Finding Charts

Helm comes with a powerful search command. It can be used to search two different types of source:

- `helm search hub` searches the Artifact Hub, which lists helm charts from dozens of different repositories.
- `helm search repo <NAME>` searches the repositories that you have added to your local helm client (with helm repo add). This search is done over local data, and no public network connection is needed.

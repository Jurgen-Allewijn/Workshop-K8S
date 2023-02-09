# Uninstall

## (Warning: Additional step) Uninstall Docker Engine

Uninstall the Docker Engine, CLI, and Containerd packages:

```bash
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io
```

Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

```bash
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

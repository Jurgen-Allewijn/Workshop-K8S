# On Node-0

Create `.ssh` dir in your homedir if not exists:

```
mkdir ~/.ssh
```



 For this workshop a key pair is created which is only valid today. Add the content below to a new ~/.ssh/id_rsa file

```
 vi ~/.ssh/id_rsa
```
```

```

Make readable only for user:

```
chmod 400 ~/.ssh/id_rsa
```

Clone git repository on the Google Cloud Shell machine:

```
git clone git@gitlab.com:cloud-modern-datacenter/training/kubernetes-workshop.git ~/kubernetes-workshop
```

Goto the workshop directory:

```
cd ~/kubernetes-workshop
```

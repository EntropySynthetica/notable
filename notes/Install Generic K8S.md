---
tags: [Kubernetes]
title: Install Generic K8S
created: '2020-01-30T18:30:53.060Z'
modified: '2020-02-02T18:14:42.145Z'
---

# Install Generic K8S

Nodes are assumed to be Ubuntu 18.04 servers.
Docker is required to be installed on server ahead of time. 

kubeadm automates the setup of the cluster
kubelet is the agent that interfaces into docker
kubectl is the CLI interface into kubernetes



## Install K8S manaement apps on master and all nodes. 

### Add Google Kubernetes GPG key into APT
`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`

#### Add Kubernetes Repo into Apt
```
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

```
sudo apt update
sudo apt install kubelet kubeadm kubectl
```

### Install a specific version of kubernetes tools
`sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00`

`sudo apt-mark hold kubelet kubeadm kubectl`


## Bootstrap the cluster with Kubeadm

### Disable Swap if not already disabled.
`sudo swapoff -a `
`sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab`

### Init Cluster
On master node run
`sudo kubeadm init --pod-network-cidr=10.244.0.0/16`

### Setup kubectl 
After this is done the kube config that kubectl needs can be found at /etc/kubernetes/admin.conf

Make a directory in your home for your kube config to live in. 
`cd ~ && mkdir .kube`

Copy this file to `~/.kube/config`
change ownership of the file to your user with 
`sudo chmod <yourusername>:<yourusername> ~/.kube/config`

Test kubectl works with 
`kubectl version`

On additional nodes provision them with the following command.  The token and hash will be output from the kubeadm init join command that was run on the master.
The token can also be retrieved by running this on the master: `kubeadm token create --print-join-command`
`sudo kubeadm join $some_ip:6443 --token $some_token --discovery-token-ca-cert-hash $some_hash`

Verify nodes have joined with 
`kubectl get nodes`

It's ok if they comeup in a not ready status.  That will be resolved when networking is setup in the next step. 



## Setup Networking in the cluster.  

### On all Nodes run the following to enable Kubernetes to manage IPTables. 

```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Using kubectl add flannel to the cluster.  The following YAML will grab the correct containers and configure it. 
`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml`

or grab the latest version of the flannel installer
`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`

### Verify nodes are now ready
`kubectl get nodes`

It should look something like this
```
NAME                      STATUS     ROLES    AGE     VERSION
kubemaster   Ready      master   5m17s   v1.12.2
kubenode01   Ready      <none>   53s     v1.12.2
kubenode02   Ready      <none>   31s     v1.12.2
```

### Verify Flannel pods are up and running
`kubectl get pods -namespace kube-system`


The basic cluster is now setup. 


## Add Ingress Controller

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.26.2/deploy/static/mandatory.yaml`

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml`


---
tags: [Kubernetes]
title: RKE_Install
created: '2020-01-30T20:16:15.739Z'
modified: '2020-02-01T00:42:16.887Z'
---

# RKE Install
Created Tuesday 07 January 2020

Kubectl install on any host that will manage the cluster

```
curl -LO <https://storage.googleapis.com/kubernetes-release/release/`curl> -s <https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl>
chmod +x kubectl
sudo mv kubectl [/usr/local/bin/](file:///usr/local/bin)
```

To connect to a cluster get it's kubeconfig file and store it somewhere.  The default search path is [~/.kube/config](file:///home/erica/.kube/config)
To use a differnt file run
set KUBECONFIG [./kube/config.<clustername](./RKE_Install/kube/config.<clustername)>.yml

Install Docker on the nodes
```
curl -fsSL <https://get.docker.com> -o get-docker.sh
sudo sh [./get-docker.sh](./RKE_Install/get-docker.sh)
sudo usermod -aG docker your-user
```

Get RKE binary installed on a server that is not one of the nodes. 
```
curl -LO <https://github.com/rancher/rke/releases/download/v1.0.0/rke_linux-amd64>
chmod +x rke_linux-amd64
sudo cp rke_linux-amd64 /usr/local/bin/rke
rke --version
```

Make RKE config file.

rke config --name cluster.yml

```
erica@kubemaster:~$ rke config --name cluster.yml
[+] Cluster Level SSH Private Key Path [~/.ssh/id_rsa]: 
[+] Number of Hosts [1]: 2
[+] SSH Address of host (1) [none]: 192.168.68.101
[+] SSH Port of host (1) [22]: 
[+] SSH Private Key Path of host (192.168.68.101) [none]: ~/.ssh/id_rsa
[+] SSH User of host (192.168.68.101) [ubuntu]: erica
[+] Is host (192.168.68.101) a Control Plane host (y/n)? [y]: y
[+] Is host (192.168.68.101) a Worker host (y/n)? [n]: y
[+] Is host (192.168.68.101) an etcd host (y/n)? [n]: y
[+] Override Hostname of host (192.168.68.101) [none]: node01
[+] Internal IP of host (192.168.68.101) [none]: 
[+] Docker socket path on host (192.168.68.101) [/var/run/docker.sock]: 
[+] SSH Address of host (2) [none]: 192.168.68.102
[+] SSH Port of host (2) [22]: 
[+] SSH Private Key Path of host (192.168.68.102) [none]: ~/.ssh/id_rsa
[+] SSH User of host (192.168.68.102) [ubuntu]: erica
[+] Is host (192.168.68.102) a Control Plane host (y/n)? [y]: y
[+] Is host (192.168.68.102) a Worker host (y/n)? [n]: y
[+] Is host (192.168.68.102) an etcd host (y/n)? [n]: y
[+] Override Hostname of host (192.168.68.102) [none]: node02
[+] Internal IP of host (192.168.68.102) [none]: 
[+] Docker socket path on host (192.168.68.102) [/var/run/docker.sock]: 
[+] Network Plugin Type (flannel, calico, weave, canal) [canal]: 
[+] Authentication Strategy [x509]: 
[+] Authorization Mode (rbac, none) [rbac]: 
[+] Kubernetes Docker image [rancher/hyperkube:v1.16.3-rancher1]: 
[+] Cluster domain [cluster.local]: 
[+] Service Cluster IP Range [10.43.0.0/16]: 
[+] Enable PodSecurityPolicy [n]: 
[+] Cluster Network CIDR [10.42.0.0/16]: 
[+] Cluster DNS Service IP [10.43.0.10]: 
[+] Add addon manifest URLs or YAML files [no]: 
```

Save a copy of the following files in a secure location (they will be output by the tool):

cluster.yml: The RKE cluster configuration file.
kube_config_cluster.yml: The Kubeconfig file for the cluster, this file contains credentials for full access to the cluster.
cluster.rkestate: The Kubernetes Cluster State file, this file contains credentials for full access to the cluster.


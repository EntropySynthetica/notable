---
tags: [Kubernetes]
title: Kubernetes Lab Setup
created: '2020-08-26T16:12:04.379Z'
modified: '2020-08-26T19:37:12.426Z'
---

# Kubernetes Lab Setup

The following will detail creating a Kubernetes lab and setting up a test workload

Kubernetes is an open-source container orchestration platform that automates the deployment, management, scaling, and networking of containers. It was developed by Google using the Go Programming Language.

This lab will be setup using K3S.  

## Requirements

* Ubuntu Linux 16.04 or newer.
* Firewall open TCP ports 80, 443, 6443, and 30000 to 32600

## Install Prerequisite packages. 

`sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common`

## Install Docker

### Get the Docker GPG key and add it to apt
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

### Add the Docker Repo
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### Install Docker
`sudo apt update`
`sudo apt install docker-ce`


## Verify Docker is running

### Add your user to docker
`sudo usermod -aG docker your-user`

You will need to log out and back in after adding yourself to the group. 

Check Docker Version
`docker --version`

Show running Containers
`docker ps -a`

## Install Kubectl

### Add Google Kubernetes GPG key
`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`

### Add the Google Kubernetes Repo
`echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list`

### Install kubectl
`sudo apt update`
`sudo apt install kubectl`

### Verify kubectl works
`kubectl version --client`


## Install K3S

Download the K3s Binary, the links can be found at https://github.com/rancher/k3s/releases/latest
example:
`curl -OL https://github.com/rancher/k3s/releases/download/v1.18.8%2Bk3s1/k3s`

Move K3S to the proper directory
`chmod +x k3s `
`sudo mv k3s /usr/local/bin`

Test with 
`k3s --version`

Start K3s with 
`sudo k3s server --docker &`

## Setup kubectl to talk to the cluster

Once K3S is up and running it will generate a config file at /etc/rancher/k3s/k3s.yaml
To get kubectl to talk to k3s we need to feed this config file to kubectl.  

Switch to your home directory
`cd ~`

Make a directory named .kube
`mkdir .kube`

Copy the kube config file into the .kube folder 
`sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config`

fixup the permissions on the config file

`sudo chown sysadmin:sysadmin ~/.kube/config`

Verify kubectl can see the cluster with 
`kubectl get nodes`

## Add a deployment to the Kubernetes cluster

Create a deployment using a test webserver.
`kubectl create deployment demo --image=monachus/rancher-demo:latest`

Verify the deployment is running
`kubectl get deploy`

## Expose the deployment using a service. 

Expose the deployment using a nodeport service
`kubectl create service nodeport demo --node-port=31337 --tcp=8080:8080`

Verify the Service exists
`kubectl get service`

Point your web browser to http://<node IP>:31337 to access the workload.

## Scale up the service. 
`kubectl scale deployments/demo --replicas=4`









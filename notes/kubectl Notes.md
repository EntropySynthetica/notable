---
tags: [Kubernetes]
title: kubectl Notes
created: '2020-01-30T19:12:41.603Z'
modified: '2020-02-07T03:08:29.542Z'
---

# kubectl Notes

Created Monday 27 January 2020

## Install kubectl manually
Download the latest kubectl
`curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl`

or grab a specific version
`curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.17.0/bin/linux/amd64/kubectl`

make the kubectl binary executable
`chmod +x ./kubectl`

Move it to /usr/local/bin
`sudo mv ./kubectl /usr/local/bin/kubectl`

Check what version of kubectl you have
`kubectl version --client`

Check what versionof kubectl and the version of k8s you are managing
`kubectl version`

## Install kubectl via APT
```
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

## kubectl config file
Default kubectl looks for a config in `~/.kube/config`

It will also look for a config file in the enviromental variable $KUBECONFIG

You can set the filename to lookfor in ~/.kube/ with the following command
`export KUBECONFIG=config.clustername.yaml`

Multiple sites can be chained together seperated by a :
`export KUBECONFIG=$KUBECONFIG:config.site1.yaml:config.site2.yaml`

Bash script to automatically add any .yaml files in the .kube directory into the KUBECONFIG var
```
#!/bin/bash
export KUBECONFIG=$(echo $(find ~/.kube -type f -name config.\*.yaml) | sed 's/:space:/:/g')
```

```
for f in `ls ~/.kube/ | grep config.\*.yaml`
do 
    export KUBECONFIG="$HOME/.kube/$f:$KUBECONFIG"; 
done
```

## Install kubectx and kubens
kubectx is a bash script that gives a quicky way to change context
kubens is a bash script that gives a quick way to change namespace
Repo at: https://github.com/ahmetb/kubectx

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubecontext
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

Optional Install the package fzf to get the program to be interactive
`sudo apt install fzf`

Add Alias for kubectl
`alias kc=kubectl`

Check cluster info
`kubectl cluster-info`

hecking and changing kubectl contexts
`kubectl tracks multiple clusters with a context name.  The name of these can be found and set in the config.sitename.yaml files.  `

show all contexts
`kubectl config get-contexts`

show current set context
`kubectl config current-context`

change current context
`kubectl config set-context <context_name>`

Get Namespaces in a Cluster
`kubectl get all --all-namespaces`

Set Default Namespace
`kubectl config set-context --current --namespace=<insert-namespace-name-here>`

Show Default Namespace
`kubectl config view --minify | grep namespace:`

### Create a workload
`kubectl create deployment nginx --image=nginx:1.14`
or
`kubectl create deployment helloworld --image=rancher/hello-world`
or
`kubectl create deployment demo --image=monachus/rancher-demo:latest`

### Upgrade the image a deployment uses
`kubectl set image deploy nginx nginx=nginx:1.15 --record`

the --record in the previous command allows rollback. 

Verify with
`kubectl get pods`
or
`kubectl get all`

### Expose workload via a service
`kubectl expose deployment nginx --port 80 --target-port=8000 --name=nginx-http`
or
`kubectl expose deployment helloworld --type="NodePort"  --port 80  --name=helloworld-http`
or
`kubectl create service nodeport helloworld --node-port=31337 --tcp80:80`
or
`kubectl expose rc nginx --port=80 --target-port=8000`

`kubectl expose deployment.apps/nginx --type="NodePort" --port 80`

Verify endpoint
`k3s kubectl get endpoints mynginx`

### Scale deployments
`kubectl scale deployments/helloworld --replicas=4`

### Show what nodes the pods are on with
`kubectl get pods -o wide`

### Check resources
See node utilization
`kubectl top node`

check pod cpu utilization
`kubectl top pod --all-namespaces`

### Get YAML for a deployment
`kubectl get deploy <deployment name> -o yaml`

### Get logs for a pod
`kubectl logs -f <pod name>`

### Get events for a pod (useful for troubleshooting)
`kubectl describe pod <pod name>`

## Install SSL Cert as Secret
`kubectl create secret tls ${CERT_NAME} --key ${KEY_FILE} --cert ${CERT_FILE}`

The cert can then be referneced by the ingress controller to enable HTTPS. 


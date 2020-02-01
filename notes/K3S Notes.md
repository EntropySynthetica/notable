---
tags: [Kubernetes]
title: K3S Notes
created: '2020-01-30T18:32:37.436Z'
modified: '2020-01-30T19:09:36.714Z'
---

# K3S Notes

Created Tuesday 07 January 2020

External Notes:
https://levelup.gitconnected.com/kubernetes-cluster-with-k3s-and-multipass-7532361affa3

On Master Node
--- Install on Master Node
`curl -sfL https://get.k3s.io | sh -`

#this command installs k3s with kubectl that doesn't need to be root to run. 
`curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644`

Run with Docker as Container Runtime
`curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --docker`

---- Check status with 
`kubectl get nodes`

`kubectl describe node/<nodename>`

`k3s kubectl get node -o wide`

Get the master nodes token

`sudo cat /var/lib/rancher/k3s/server/node-token`

----Add a worker node
`curl -sfL https://get.k3s.io | K3S_TOKEN=${K3S_TOKEN} K3S_URL=${K3S_NODEIP_MASTER} sh -`

`curl -sfL https://get.k3s.io | K3S_TOKEN=K10de4da160ebd25d07e05487a03e95432b90c2d894ceb6260019780c2279dade8d::server:f26cdd5f5d958a316aa347258b02aa72 K3S_URL=https://192.168.68.130:6443 sh -`

`curl -sfL https://get.k3s.io | K3S_TOKEN=K10e8c4e0978bcc9d67c3eaf51de7576f4980609c2c67de77bb97e42d4528ea6045::server:a25e143f6524ac9c6f076ea67b028fc9 K3S_URL=https://192.168.68.100:6443 sh -s - --docker`

`sudo k3s agent --server ${K3S_URL} --token ${K3S_TOKEN}`

-----Check cluster info
`k3s kubectl cluster-info`

`k3s kubectl get all --all-namespaces`



K3s Manual Install via Docker
Make sure docker is installed on the nodes

Download the K3s Binary, the links can be found at https://github.com/rancher/k3s/releases/latest
example:
`curl -OL https://github.com/rancher/k3s/releases/download/v1.17.0%2Bk3s.1/k3s`

```
chmod +x k3s 
sudo mv k3s /usr/local/bin
```

Test with 
`k3s --version`

Start K3s Master node with
`sudo k3s server --docker &`

Get the kubeconfig file with
`cat /etc/rancher/k3s/k3s.yaml`

Get the node token with
`sudo cat /var/lib/rancher/k3s/server/node-token`

Install k3s binary on any additional nodes with same steps, but execute with
`sudo k3s agent --docker --server ${K3S_URL} --token ${K3S_TOKEN} &`


----- Add as systemd service

On the master node.
#### Copy this to /etc/systemd/system/k3s.service
```
[Unit]
Description=Lightweight Kubernetes
Documentation=https://k3s.io
Wants=network-online.target

[Install]
WantedBy=multi-user.target

[Service]
Type=notify
EnvironmentFile=/etc/systemd/system/k3s.service.env
KillMode=process
Delegate=yes
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TasksMax=infinity
TimeoutStartSec=0
Restart=always
RestartSec=5s
ExecStartPre=-/sbin/modprobe br_netfilter
ExecStartPre=-/sbin/modprobe overlay
ExecStart=/usr/local/bin/k3s \
	server \
		'--write-kubeconfig-mode' \
		'644' \
		'--docker' \
```

####

Reload systemd

`sudo systemctl daemon-reload`

check the service

`sudo service k3s status`


### On any worker nodes

### Add the following to /etc/systemd/system/k3s-agent.service.env 
### Replace with apprpriate token and URL
```
K3S_TOKEN=K101b67474c7a4ffc3a1ad970ff47e3b9ce041afc8708a44cea54f865f3a6b28e52::server:3f88109869142782e1ee156cccf84d5c
K3S_URL=https://192.168.68.100:6443
```

### /etc/systemd/system/k3s-agent.service
```
[Unit]
Description=Lightweight Kubernetes
Documentation=https://k3s.io
Wants=network-online.target

[Install]
WantedBy=multi-user.target

[Service]
Type=exec
EnvironmentFile=/etc/systemd/system/k3s-agent.service.env
KillMode=process
Delegate=yes
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TasksMax=infinity
TimeoutStartSec=0
Restart=always
RestartSec=5s
ExecStartPre=-/sbin/modprobe br_netfilter
ExecStartPre=-/sbin/modprobe overlay
ExecStart=/usr/local/bin/k3s \
	agent \
	'--docker' \
```





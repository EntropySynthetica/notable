---
tags: [Linux]
title: Gluster Install
created: '2020-07-22T21:31:44.231Z'
modified: '2020-07-23T16:37:01.758Z'
---

# Gluster Install

## Server Setup

### Run on all nodes

`sudo add-apt-repository ppa:gluster/glusterfs-8`

`sudo apt update`

`apt install glusterfs-server`

Setup Hosts

```
192.168.0.11    labgluster01.lab
192.168.0.12    labgluster02.lab
192.168.0.13    labgluster03.lab

```

Setup Firewall

```
# Open Gluster Ports
iptables -A INPUT -p tcp --dport 24007:24008 -m state --state NEW -j ACCEPT
iptables -A INPUT -p tcp --dport 49152:49156 -m state --state NEW -j ACCEPT

```


### Run on Client

`sudo add-apt-repository ppa:gluster/glusterfs-8`

`sudo apt update`

`sudo apt install glusterfs-client`

### Mount on Client

`sudo mount -t glusterfs labgluster01.lab:/gv0 /data`

### Show mounted clients
`gluster volume status <vol_name> clients`

### Get Volume Settings
`gluster volume info`

### Set the ping timeout for a failed of shutdown server. 
`gluster volume set <vol_name> network.ping-timeout <seconds>` # Default 42 Seconds

### Shell script to do a safe shutdown of the server
https://github.com/gluster/glusterfs/blob/master/extras/stop-all-gluster-processes.sh

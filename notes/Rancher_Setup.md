---
tags: [Kubernetes]
title: Rancher_Setup
created: '2020-01-30T20:16:15.731Z'
modified: '2020-01-30T20:49:42.107Z'
---

# Rancher Setup
Created Tuesday 29 October 2019


To disable ubuntu’s default dns server to free up port 53, I ended up doing the following

Add ‘127.0.0.1 localhost’ to /etc/hosts file
update /etc/resolv.conf to only include:
nameserver 8.8.8.8
nameserver 8.8.4.4
Run the following:
sudo systemctl disable systemd-resolved.service
sudo service systemd-resolved stop


rancher/hello-world


sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 -v /opt/rancher:/var/lib/rancher rancher/rancher:v2.3.5

docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /opt/rancher:/var/lib/rancher \
  -v /opt/certs/cert.pem:/etc/rancher/ssl/cert.pem \
  -v /opt/certs/key.pem:/etc/rancher/ssl/key.pem \
  -v /opt/certs/cacert.pem:/etc/rancher/ssl/cacerts.pem \
  rancher/rancher:v2.3.5

wget <https://certs.godaddy.com/repository/gd_bundle.crt>





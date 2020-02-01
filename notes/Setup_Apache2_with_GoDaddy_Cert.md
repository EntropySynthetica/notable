---
tags: [Linux]
title: Setup_Apache2_with_GoDaddy_Cert
created: '2020-01-30T20:16:15.911Z'
modified: '2020-01-30T20:53:26.626Z'
---

# Setup Apache2 with GoDaddy Cert
Created Monday 21 May 2018

Three files will be needed.  

Private Key, generated on the web server.  

Signed Cert, returned from the CA

CA Intermedate Cert, can be Downloaded from <https://certs.godaddy.com/repository/gd_bundle-g2-g1.crt>

Assuming the following file names,

Priv Key = priv.key
Cert = mycert.crt
CA = gd_bundle-g2-g1.crt

Create a PEM file from these 2,

cat mycert.crt gd_bundle-g2-g1.crt > mycert.pem

In the Conf file for the site make sure to add the following lines, 

SSLCertificateFile /etc/apache2/ssl/mycert.pem
SSLCertificateKeyFile [/etc/apache2/ssl/priv.key](file:///etc/apache2/ssl/priv.key)

reload apache with

sudo service apache2 reload

Site can be verified with

openssl s_client -showcerts -connect [www.mysite.com:443](./www.mysite.com/443.markdown)

openssl verify -CAfile chain.pem mycert.pem


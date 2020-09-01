---
tags: [Kubernetes]
title: Hasicorp Vault Notes
created: '2020-08-31T01:45:19.253Z'
modified: '2020-08-31T18:49:58.948Z'
---

# Hasicorp Vault Notes


## Setup SSH Signed Certificates

Mount the secrets engine.
```
vault secrets enable -path=ssh-client-signer ssh
```

Create a key pair
```
vault write ssh-client-signer/config/ca generate_signing_key=true
```

Add the key to the hosts SSH config

```
curl -o /etc/ssh/trusted-user-ca-keys.pem http://<vault IP>:8200/v1/ssh-client-signer/public_key
```

Add the following line to the end of `/etc/ssh/sshd_config`

```
TrustedUserCAKeys /etc/ssh/trusted-user-ca-keys.pem
```

Restart the SSH service on the host. 


Create a Vault role for signing keys

```
vault write ssh-client-signer/roles/my-role -<<"EOH"
{
  "allow_user_certificates": true,
  "allowed_users": "*",
  "allowed_extensions": "permit-pty,permit-port-forwarding",
  "default_extensions": [
    {
      "permit-pty": ""
    }
  ],
  "key_type": "ca",
  "default_user": "sysadmin",
  "ttl": "5m0s"
}
EOH
```

On your client ask vault to generate and save a signed key you can use to log into the host. 

```
vault write -field=signed_key ssh-client-signer/sign/my-role public_key=~/.ssh/id_rsa.pub > ~/.ssh/id_rsa-cert.pub
```

You can now SSH into the host until the TTL expires. 

You can verify the signed cert with the following command

```
ssh-keygen -Lf ~/.ssh/id_rsa-cert.pub
```


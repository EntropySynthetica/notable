---
tags: [Crypto, Linux]
title: SSH_Keys
created: '2020-01-30T20:16:15.923Z'
modified: '2020-02-12T21:27:47.189Z'
---

# SSH Keys
Created Monday 05 June 2017

RSA Private Key stored in `~./ssh/id_rsa`
RSA Public Key stored in `~./ssh/id_rsa.pub`

### To generate a new key run
`ssh-keygen -t rsa`

### To SSH in to a remote system without a password:
Copy the data in the pub file to a new line in  `~/.ssh/authorized_keys` on the remote system. 
Make sure .ssh/ and authorized_keys is owned by the username, and group is the username
Run chmod 700 on `~/.ssh` and chmod 600 on authorized_keys file.

Comments can be added to authorized_keys file on lines starting with #

You can also use the `ssh-copy-id` command to automatically do this. 
By default the linux command ssh-copy-id will look for the public key in 
`~/.ssh/identity.pub`

By defualt the linux ssh client will use `~/.ssh/id_rsa` as a private key.

### To get the public key from the private key run 
`ssh-keygen -y -f ~/.ssh/id_rsa`

### To manually point SSH to a private key to use  
`ssh -i ~/.ssh/id_rsa.otherkey user@server.example.com`


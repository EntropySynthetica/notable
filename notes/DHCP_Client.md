---
tags: [Linux]
title: DHCP_Client
created: '2020-01-30T20:16:15.491Z'
modified: '2020-01-30T20:39:41.721Z'
---

# DHCP Client
Created Thursday 11 May 2017


Linux renew ip command
The -r flag explicitly releases the current lease, and once the lease has been released, the client exits.

sudo dhclient -4 -r -v

Now obtain fresh IP:

sudo dhclient -4 -d -v

To renew or release an IP address for the eth0 interface, enter:

sudo dhclient -4 -r -v eth0
sudo dhclient -4 -d -v eth0

The -v option shows information on screen about dhcp server and obtained lease.



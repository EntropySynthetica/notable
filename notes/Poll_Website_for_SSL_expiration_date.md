---
tags: [Linux]
title: Poll_Website_for_SSL_expiration_date
created: '2020-01-30T20:16:15.859Z'
modified: '2020-01-30T20:47:17.038Z'
---

# Poll Website for SSL expiration date
Created Friday 04 May 2018

openssl s_client -connect www.entropycraft.com:443 < /dev/null 2>/dev/null | openssl x509 -noout -enddate

